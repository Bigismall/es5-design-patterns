# Singleton

Wzorzec singleton ma w założeniu zapewnić tylko jedną instancję danej klasy.  Oznacza to, że próba utworzenia obiektu danej klasy po raz drugi powinna zwrócić dokładnie ten sam obiekt, który został zwrócony za pierwszym razem.

## EcmaScript 6 implementation \(poor\)

```js
{
    let _singletoneInstance;        //do not var

    function SolwitSingleton(name) {
        if (!_singletoneInstance) {
            this.name = name;
            _singletoneInstance = this;
        }
        return _singletoneInstance;
    }

}
```

[https://codepen.io/Bigismall/pen/EXmNOJ](https://codepen.io/Bigismall/pen/EXmNOJ)

## EcmaScript 5 implementation

```js
var SolwitSingleton = (function () {
    var _singletoneInstance;

    return function SolwitSingleton(name) {
        if (!_singletoneInstance) {
            this.name = name;
            _singletoneInstance = this;
        }
        return _singletoneInstance;
    }

}());
```

[https://codepen.io/Bigismall/pen/yXbVWR](https://codepen.io/Bigismall/pen/yXbVWR)







