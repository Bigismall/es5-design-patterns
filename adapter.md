# Adapter

Sporadycznie pojawia się potrzeba dopasowania okrągłego kołka do kwadratowego otworu.

Klasa adaptera to niewielka porcja kodu implementującego wymagany interfejs. Kod ten opakowuje zwykle kopię **prywatną** klasy implementacji i pośredniczy w przekazywaniu wywołań do niej.

Wzorzec adapter jest często używany do zmiany poziomu abstrakcji kodu.

| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |





```js
const HPA_IN_MMHG = 0.7500617;

class Thermometer {
    constructor() {
        this.temperature = -30 + Math.round(60 * Math.random());
        this.pressure = 700 + Math.round(200 * Math.random());
    }

    getTemperature() {
        return this.temperature;
    }

    getPressure() {
        return this.pressure;
    }

    info() {
        return `The temperature is  ${this.getTemperature()} F  and pressure  ${this.getPressure()} mmHg`;
    }
}

class EuThermometerAdapter {
    constructor() {
        this.thermometer = new Thermometer();
    }

    getCelsiusTemperature() {
        return Math.round(5 / 9 * (this.thermometer.getTemperature() - 32));
    }

    getHgPressure() {
        return Math.round(this.thermometer.getPressure() * HPA_IN_MMHG);
    }

    temperatureInfo() {
        return `The temperature is  ${this.getCelsiusTemperature()} C`;
    }

    pressureInfo() {
        return `The pressure is ${this.getHgPressure()} hPa`;
    }
}
```

[https://codepen.io/Bigismall/pen/YQVNvr](https://codepen.io/Bigismall/pen/YQVNvr)



