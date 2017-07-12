# Pośrednik

We wzorcu projektowym pośrednika jedne obiekt stanowi interfejs dla innego obiektu.  Pośrednik znajduje się między użytkownikiem a obiektem i broni dostępu do niego. Choć wzorzec wygląda jak dodatkowy narzut, w rzeczywistości często służy do poprawy wydajności. Pośrednik może być przydatny w przypadkach:

* leniwe tworzenie instancji kosztownego obiektu
* ochrona danych poufnych
* symulowanie zachowania wywołania metody zdalnej \(middleware\)
* wprowadzenie dodatkowych działań przed wywołaniem metody lub po nim

```js
var API_URL = "https://api.giphy.com/v1/gifs/search?api_key=dc6zaTOxFJmzC&limit=1&offset=0";
var API_QUERY = "&q=";


function Subject() {
    this.request = function (query) {
        throw Error("Not implemented");
    }
}

function RealSubject() {
    Subject.call(this);

    this.request = function (query) {
        return fetch(API_URL + API_QUERY + encodeURIComponent(query))
            .then(function (response) {
                return response.json();
            });
    }
}

RealSubject.prototype = Object.create(Subject.prototype);
RealSubject.prototype.constructor = Subject;


function Proxy() {
    Subject.call(this);
    this.cache = {};

    this.request = function (query) {
        console.log('PROXY request for QUERY: ', query);

        if (this.cache.hasOwnProperty(query)) {
            return this.cache[query];
        } else {
            var realSubject = new RealSubject(),
                that = this;

            realSubject
                .request(query)
                .then(function (body) {
                    that.cache[query] = body.data[0]['embed_url'];
                    return that.cache[query];
                });
        }
    }

}

Proxy.prototype = Object.create(Subject.prototype);
Proxy.prototype.constructor = Subject;


var proxy = new Proxy();
proxy.request("plane");

window.setTimeout(function () {
    return proxy.request("truck");
}, 3000);

window.setTimeout(function () {
    return proxy.request("plane");
}, 6000);

window.setTimeout(function () {
    return proxy.request("truck");
}, 9000);

window.setTimeout(function () {
    return proxy.request("plane");
}, 12000);

window.setTimeout(function () {
    return console.log(proxy.cache);
}, 15000);
```

[https://codepen.io/collection/DBQYvr/](https://codepen.io/collection/DBQYvr/)

