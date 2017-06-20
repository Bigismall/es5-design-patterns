# Pośrednik

sdadsa

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

