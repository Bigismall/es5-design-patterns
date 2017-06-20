# Pośrednik

We wzorcu projektowym pośrednika jedne obiekt stanowi interfejs dla innego obiektu.  Pośrednik znajduje się między użytkownikiem a obiektem i broni dostępu do niego. Choć wzorzec wygląda jak dodatkowy narzut, w rzeczywistości często służy do poprawy wydajności. Pośrednik może być przydatny w przypadkach:

* leniwe tworzenie instancji kosztownego obiektu
* ochrona danych poufnych
* symulowanie zachowania wywołania metody zdalnej \(middleware\)
* wprowadzenie dodatkowych działań przed wywołaniem metody lub po nim



```js
class Subject {
    constructor() {
    }

    request(query) {
        return JSON.parse("{}");
    }
}

class RealSubject extends Subject {
    constructor() {
        super();
    }

    request(query) {
        return fetch(API_URL + API_QUERY + encodeURIComponent(query))
            .then(response => response.json());
    }
}

class Proxy extends Subject {
    constructor() {
        super();
        this.cache = {};
    }

    request(query) {
        console.log('PROXY request for QUERY: ', query);
        if (this.cache.hasOwnProperty(query)) {
            return this.cache[query];
        } else {
            const realSubject = new RealSubject();
            realSubject
                .request(query)
                .then(body => {
                    this.cache[query] = body.data[0]['embed_url'];
                    return this.cache[query];
                });
        }
    }
}
```

[https://codepen.io/Bigismall/pen/rwjwWe](https://codepen.io/Bigismall/pen/rwjwWe)

