# Fabryka \(factory method\)

Celem wzorca fabryki jest tworzenie obiektów. Najczęściej fabryką jest klasa lub metoda statyczna klasy, której celem jest:

* wykonanie powtarzających się operacji przy tworzeniu podobnych obiektów
* zapewnienie użytkownikom możliwości tworzenia obiektów bez potrzeby znania konkretnego typu \(klasy\) na etapie kompilacji

Obiekty tworzone przez metodę fabryczną z reguły dziedziczą po tym samym przodku, ale z drugiej strony są wyspecjalizowanymi wersjami z pewnymi dodatkowymi rozwiązaniami. Czasem wspólny przodek  to klasa zawierająca metodę fabryczną.

```js
// parent constructor
function CarMaker() {
}

// a method of the parent
CarMaker.prototype.drive = function () {
    return "Vroom, I have " + this.doors + " doors";
};


// the static factory method
CarMaker.factory = function (type) {
    var constr = type,
        newcar;

    // error if the constructor doesn't exist
    if (typeof CarMaker[constr] !== "function") {
        throw {
            name: "Error",
            message: constr + " doesn't exist"
        };
    }

    // at this point the constructor is known to exist
    // let's have it inherit the parent but only once
    if (typeof CarMaker[constr].prototype.drive !== "function") {
        CarMaker[constr].prototype = new CarMaker();
    }

    // create a new instance
    newcar = new CarMaker[constr]();
    // optionally call some methods and then return...
    return newcar;
};


// define specific car makers
CarMaker.Compact = function () {
    this.doors = 4;
};
CarMaker.Convertible = function () {
    this.doors = 2;
};
CarMaker.SUV = function () {
    this.doors = 24;
};


var corolla = CarMaker.factory('Compact');
var solstice = CarMaker.factory('Convertible');
var cherokee = CarMaker.factory('SUV');

console.log(corolla.drive()); // "Vroom, I have 4 doors"
console.log(solstice.drive()); // "Vroom, I have 2 doors"
console.log(cherokee.drive()); // "Vroom, I have 24 doors"
```

**Zastosowanie:** zwracanie odpowiedniej funkcji sortującej w zależności od typu danych

