# Obserwator

Głównym celem używania wzorca jest promowanie **luźnego powiązania** elementów. Zamiast sytuacji w której jeden obiekt wywołuje metodę drugiego, mamy sytuację  w której drugi z obiektów zgłasza chęć otrzymywania powiadomień o zmianie w pierwszym obiekcie. Subskrybenta nazywa się  często **obserwatorem**, a obiekt obserwowany obiektem **publikującym **lub źródłem. Obiekt publikujący wywołuje subskrybentów po zajściu istotnego zdarzenie i bardzo często przekazuje informację o nim w postaci obiektu zdarzenia.

```js
class Publisher {
    constructor() {
        this.subscribers = [];
    }

    subscribe(callback) {
        this.subscribers.push(callback);
    }

    unsubscribe(callback) {
        this.subscribers = this.subscribers.filter((s) => s !== callback);
    }

    publish(publication) {
        this.subscribers.forEach((s) => s.update(publication));
    }
}

class Observer {
    constructor() {
    }

    update(publication) {
        console.log(`Publication: ${publication}`);
    }
}


class Newspaper extends Publisher {
    constructor() {
        super();
    }

    daily(message) {
        this.publish(message);
    }
}


class Reader extends Observer {
    constructor(name) {
        super();
        this.name = name;
    }

    update(publication) {
        console.log(`Nazywam się ${this.name} i czytam sobie na klopie: ${publication}`);
    }
}


let newsPaper = new Newspaper(),
    janusz = new Reader('Janusz'),
    grazyna = new Reader('Grażyna'),
    leszek = new Reader('Leszek'),
    mateusz = new Observer();


newsPaper.subscribe(janusz);
newsPaper.subscribe(grazyna);
newsPaper.subscribe(leszek);
newsPaper.subscribe(mateusz);


newsPaper.daily("Jurorka 'Supermodelki Plus Size' chorowała na bulimię! 'Nienawidziłam swojego ciała'");
newsPaper.unsubscribe(leszek);
newsPaper.daily("Ziobro chce zmienić prawo, żeby homofobiczna dyskryminacja była legalna!");
```

[https://codepen.io/Bigismall/pen/GEmEaG](https://codepen.io/Bigismall/pen/GEmEaG)

Nic nie stoi na przeszkodzie aby obserwator był zarazem obiektem publikującym i na odwrót aby obiekt publikujący był zarazem obserwatorem. W rezultacie mamy  2 kierunkową komunikację luźno powiązanych obiektów.

[https://codepen.io/Bigismall/pen/owWevG](https://codepen.io/Bigismall/pen/owWevG)

