# Singleton

Wzorzec singleton ma w założeniu zapewnić tylko jedną instancję danej klasy.  Oznacza to, że próba utworzenia obiektu danej klasy po raz drugi powinna zwrócić dokładnie ten sam obiekt, który został zwrócony za pierwszym razem.

## EcmaScript 5 implementation

```js
function Universe(name) {
    var instance = this;

    this.name = name;
    this.start_time = Date.now();

// rewrite the constructor
    Universe = function () {
        return instance;
    };
}


var uni1 = new Universe("1");
var uni2 = new Universe("2");

console.log(uni1);                  //Universe { start_time: 1499795415812, name: '1' }
console.log(uni2);                  //Universe { start_time: 1499795415812, name: '1' }
console.log(uni1 === uni2);         // true

```



