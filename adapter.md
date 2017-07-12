# Adapter

Sporadycznie pojawia się potrzeba dopasowania okrągłego kołka do kwadratowego otworu.

Klasa adaptera to niewielka porcja kodu implementującego wymagany interfejs. Kod ten opakowuje zwykle kopię prywatną klasy implementacji i pośredniczy w przekazywaniu wywołań do niej.

Wzorzec adapter jest często używany do zmiany poziomu abstrakcji kodu.

| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |



```js
function Ship() {
    this.setRudderAngleTo = function (angle) {
        console.log('setRudderAngleTo');
    };

    this.setSailConfiguration = function (configuration) {
        console.log('setSailConfiguration');
    };

    this.setSailAngle = function (sailId, sailAngle) {
        console.log('setSailAngle');
    };

    this.getCurrentBearing = function () {
        console.log('getCurrentBearing');
        return Math.random() * 360;
    };

    this.getCurrentSpeedEstimate = function () {
        console.log('getCurrentSpeedEstimate');
        return Math.random() * 30;
    };

    this.shiftCrewWeightTo = function (weightToShift, locationId) {
        console.log('shiftCrewWeightTo');
    };
}


function SimpleShipAdapter() {
    this._ship = new Ship();

    this.turnLeft = function () {
        this._ship.setRudderAngleTo(-30);
        this._ship.setSailAngle(3, 12);
    };

    this.turnRight = function () {
        this._ship.setRudderAngleTo(30);
        this._ship.setSailAngle(5, -9);
    };

    this.goForward = function () {
        this._ship.setSailConfiguration({});
        this._ship.shiftCrewWeightTo(1000, 'back');
        this._ship.setSailAngle(0, 90);
    }
}


var ship = new SimpleShipAdapter();

ship.goForward();
ship.turnLeft();

```



