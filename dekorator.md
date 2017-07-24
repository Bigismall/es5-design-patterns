# Dekorator

Wzorzec dekorator służy do opakowywanie i rozszerzania istniejącej klasy. Użycie tego wzorca stanowi alternatywę wobec definiowania podklasy w przypadku istniejącego komponentu.

Cechą wzorca dekoratora jest łatwość dostosowania i konfiguracji oczekiwanego zachowania. Zaczyna się od prostego obiektu z podstawową funkcjonalnością. Następnie wybiera się kilka z zestawu dostępnych dekoratorów, po czym rozszerza się nimi podstawowy obiekt. Czasem istotna jest kolejność tego rozszerzania.

| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |

```js
function ISell() {
    this.getPrice = function () {
        throw Error("Not implemented");
    }
}


function Sell(name, price) {
    ISell.call(this);
    this.name = name;
    this.price = price;

    this.getPrice = function () {
        return this.price;
    }
}

Sell.prototype = Object.create(ISell.prototype);
Sell.prototype.constructor = Sell;


function VAT(sell) {
    ISell.call(this);

    this.sell = sell;
    this.rate = 25;

    this.getPrice = function () {
        return this.sell.getPrice() * (1 + this.rate / 100);
    }
}

VAT.prototype = Object.create(ISell.prototype);
VAT.prototype.constructor = VAT;


function HET(sell) {//HigherEducationTax
    ISell.call(this);

    this.sell = sell;
    this.rate = 10;

    this.getPrice = function () {
        return this.sell.getPrice() * (1 + this.rate / 100);
    }
}

HET.prototype = Object.create(ISell.prototype);
HET.prototype.constructor = HET;

var sell = new Sell('Books', 100);
var sellWithVat = new VAT(sell);
var sellWithVatAndHet = new HET(new VAT(sell));

console.log('Sell price: ', sell.getPrice());                                //100
console.log('Sell with VAT price: ', sellWithVat.getPrice());                //125
console.log('Sell with VAT and HET price: ', sellWithVatAndHet.getPrice());  //137.5
```

[https://codepen.io/Bigismall/pen/EXrxEQ](https://codepen.io/Bigismall/pen/EXrxEQ)

