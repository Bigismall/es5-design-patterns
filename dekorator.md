# Dekorator

Wzorzec dekorator służy do opakowywanie i rozszerzania istniejącej klasy. Użycie tego wzorca stanowi alternatywę wobec definiowania podklasy w przypadku istniejącego komponentu.

Cechą wzorca dekoratora jest łatwość dostosowania i konfiguracji oczekiwanego zachowania. Zaczyna się od prostego obiektu z podstawową funkcjonalnością. Następnie wybiera się kilka z zestawu dostępnych dekoratorów, po czym rozszerza się nimi podstawowy obiekt. Czasem istotna jest kolejność tego rozszerzania.

| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |



```js
class Sell {
    constructor(name = '', price = 0) {
        this.name = name;
        this.price = price;
    }

    getPrice() {
        return this.price;
    }
}


class VAT {
    constructor(sell) {
        this.sell = sell;
        this.rate = 25;
    }

    getPrice() {
        return this.sell.getPrice() * (1 + this.rate / 100);
    }
}


class HET {//HigherEducationTax
    constructor(sell) {
        this.sell = sell;
        this.rate = 10;
    }

    getPrice() {
        return this.sell.getPrice() * (1 + this.rate / 100);
    }
}


let sell = new Sell('ksiązki', 100);
let sellWithVat = new VAT(sell);
let sellWithVatAndHet = new HET(new VAT(sell));
```

[https://codepen.io/Bigismall/pen/ZyKLdb](https://codepen.io/Bigismall/pen/ZyKLdb)

