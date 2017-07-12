# Obserwator

Głównym celem używania wzorca jest promowanie **luźnego powiązania** elementów. Zamiast sytuacji w której jeden obiekt wywołuje metodę drugiego, mamy sytuację  w której drugi z obiektów zgłasza chęć otrzymywania powiadomień o zmianie w pierwszym obiekcie. Subskrybenta nazywa się  często **obserwatorem**, a obiekt obserwowany obiektem **publikującym **lub źródłem. Obiekt publikujący wywołuje subskrybentów po zajściu istotnego zdarzenie i bardzo często przekazuje informację o nim w postaci obiektu zdarzenia.

```js
function Publisher() {
    this.subscribers = [];

    this.subscribe = function (callback) {
        this.subscribers.push(callback);
    };

    this.unsubscribe = function (callback) {
        this.subscribers = this.subscribers.filter(function (s) {
            return s !== callback;
        });
    };

    this.publish = function (publication) {
        console.log("");
        this.subscribers.forEach(function (s) {
            return s.update(publication);
        });
    };
}


function Observer() {
    this.update = function (publication) {
        console.log("Publication: " + publication);
    };
}


function Newspaper() {
    Publisher.call(this);

    this.daily = function (message) {
        this.publish(message);
    };
}

Newspaper.prototype = Object.create(Publisher.prototype);
Newspaper.prototype.constructor = Newspaper;


function Reader(name) {
    Observer.call(this);

    this.name = name;
    this.update = function (publication) {
        console.log("Nazywam sie " + this.name + " i czytam sobie na klopie: " + publication);
    };
}

Reader.prototype = Object.create(Observer.prototype);
Reader.prototype.constructor = Reader;


var newsPaper = new Newspaper(),
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
newsPaper.unsubscribe(mateusz);

newsPaper.daily("Ziobro chce zmienić prawo, żeby homofobiczna dyskryminacja była legalna!");

```

[https://codepen.io/Bigismall/pen/GEmEaG](https://codepen.io/Bigismall/pen/GEmEaG)

Nic nie stoi na przeszkodzie aby obserwator był zarazem obiektem publikującym i na odwrót aby obiekt publikujący był zarazem obserwatorem. W rezultacie mamy  2 kierunkową komunikację luźno powiązanych obiektów.

[https://codepen.io/Bigismall/pen/owWevG](https://codepen.io/Bigismall/pen/owWevG)

