# Strategia

Wzorzec strategii umożliwia wybór odpowiedniego algorytmu na etapie działania aplikacji. Użytkownicy kodu mogą stosować **ten sam interfejs** zewnętrzny, ale wybierać wybierać spośród kilku dostępnych algorytmów, by lepiej dopasować implementację do aktualnego **kontekstu**.

```js
function TravelResult(name, durationInDays, probabilityOfDeath, cost) {
    this.name = name;
    this.durationInDays = durationInDays;
    this.probabilityOfDeath = probabilityOfDeath;
    this.cost = cost;
}


function TravelBase() {
    this.travel = function (source, destination) {
        throw Error("Not implemented");
    }
}


function Ship() {
    TravelBase.call(this)

    this.travel = function (source, destination) {
        this.source = source;
        this.destination = destination;
        return new TravelResult('Ship', 15, .25, 500);
    }
}

Ship.prototype = Object.create(TravelBase.prototype);
Ship.prototype.constructor = Ship;


function Horse() {
    this.travel = function (source, destination) {
        this.source = source;
        this.destination = destination;
        return new TravelResult('Horse', 30, .25, 50);
    }
}
Horse.prototype = Object.create(TravelBase.prototype);
Horse.prototype.constructor = Horse;


function Walk() {

    this.travel = function (source, destination) {
        this.source = source;
        this.destination = destination;
        return new TravelResult('Walk', 150, .55, 0);
    }
}
Walk.prototype = Object.create(TravelBase.prototype);
Walk.prototype.constructor = Walk;


function TravelStrategy() {
}

TravelStrategy.get = function (moneyAmount) {
    if (moneyAmount > 500)
        return new Ship();
    else if (moneyAmount > 50)
        return new Horse();
    else
        return new Walk();
}


var currentMoney = 1300,
    travelStrategy = TravelStrategy.get(currentMoney),
    travelResult;

travelResult = travelStrategy.travel('Danzig', 'Hamburg');
console.log(travelResult);

```



**Zastosowanie:** geolokalizacja, walidacja danych, wyświetlanie danych \(lightbox, player\)

