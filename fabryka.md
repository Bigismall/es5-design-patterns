# Fabryka \(factory method\)

Celem wzorca fabryki jest tworzenie obiektów. Najczęściej fabryką jest klasa lub metoda statyczna klasy, której celem jest:

* wykonanie powtarzających się operacji przy tworzeniu podobnych obiektów
* zapewnienie użytkownikom możliwości tworzenia obiektów bez potrzeby znania konkretnego typu \(klasy\) na etapie kompilacji

Obiekty tworzone przez metodę fabryczną z reguły dziedziczą po tym samym przodku, ale z drugiej strony są wyspecjalizowanymi wersjami z pewnymi dodatkowymi rozwiązaniami. Czasem wspólny przodek  to klasa zawierająca metodę fabryczną.

```js
class ICar {
    constructor(color = 'yellow', sits = 4, type = 'CAR') {
        this.color = color;
        this.sits = parseInt(sits, 10);
        this.type = type;
    }

    getColor() {
        return this.color;
    };

    info() {
        return "I'm a car of " + this.type + " have " + this.sits + " sits  and " + this.getColor() + " color";
    };
}


class TRUCK extends ICar {
    constructor(color = 'blue', sits = 2) {
        super(color, sits, 'TRUCK');
    }
}

class SUV extends ICar {
    constructor(color = 'silver') {
        super(color, 7, 'SUV');
    }
}

class COUPE extends ICar {
    constructor() {
        super('red', 2, 'COUPE');
    }
}


class CarFactory {

    static create(vehicleType) {
        switch (vehicleType) {
            case "TRUCK":
                return new TRUCK();
                break;
            case "SUV":
                return new SUV();
                break;
            case "COUPE":
                return new COUPE();
                break;
            default:
                return new Car()
        }
    }
}
```

[https://codepen.io/Bigismall/pen/WOjRQy](https://codepen.io/Bigismall/pen/WOjRQy)

**Zastosowanie:** zwracanie odpowiedniej funkcji sortującej w zależności od typu danych

