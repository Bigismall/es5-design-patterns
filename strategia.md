# Strategia

Wzorzec strategii umożliwia wybór odpowiedniego algorytmu na etapie działania aplikacji. Użytkownicy kodu mogą stosować **ten sam interfejs** zewnętrzny, ale wybierać wybierać spośród kilku dostępnych algorytmów, by lepiej dopasować implementację do aktualnego **kontekstu**.

```js
class TravelResult {
    constructor(durationInDays, probabilityOfDeath, cost) {
        this.durationInDays = durationInDays;
        this.probabilityOfDeath = probabilityOfDeath;
        this.cost = cost;
    }
}

class SeaGoingVessel {
    travel(source, destination) {
        this.source = source;
        this.destination = destination;
        return new TravelResult(15, .25, 500);
    }
}

class Horse {
    travel(source, destination) {
        this.source = source;
        this.destination = destination;
        return new TravelResult(30, .25, 50);
    }
}

class Walk {
    travel(source, destination) {
        this.source = source;
        this.destination = destination;
        return new TravelResult(150, .55, 0);
    }
}

class TravelStrategy {
    static get(moneyAmount) {
        if (moneyAmount > 500)
            return new SeaGoingVessel();
        else if (moneyAmount > 50)
            return new Horse();
        else
            return new Walk();
    }
}


var currentMoney = 300,
    travelStrategy = TravelStrategy.get(currentMoney),
    travelResult;

travelResult = travelStrategy.travel('Danzig', 'Hamburg');
```

[https://codepen.io/Bigismall/pen/YQVVyo](https://codepen.io/Bigismall/pen/YQVVyo)

**Zastosowanie:** geolokalizacja, walidacja danych, wyświetlanie danych \(lightbox, player\)

