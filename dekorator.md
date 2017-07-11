# Dekorator

Wzorzec dekorator służy do opakowywanie i rozszerzania istniejącej klasy. Użycie tego wzorca stanowi alternatywę wobec definiowania podklasy w przypadku istniejącego komponentu.

Cechą wzorca dekoratora jest łatwość dostosowania i konfiguracji oczekiwanego zachowania. Zaczyna się od prostego obiektu z podstawową funkcjonalnością. Następnie wybiera się kilka z zestawu dostępnych dekoratorów, po czym rozszerza się nimi podstawowy obiekt. Czasem istotna jest kolejność tego rozszerzania.

| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |

```js
function Sale(price) {
    this.price = price || 100;
}

Sale.prototype.decorate = function (decorator) {
    var F = function () {
        },
        overrides = this.constructor.decorators[decorator],
        i, newobj;

    F.prototype = this;
    newobj = new F();
    newobj.super = F.prototype;

    for (i in overrides) {
        if (overrides.hasOwnProperty(i)) {
            newobj[i] = overrides[i];
        }
    }

    return newobj;
};

Sale.prototype.getPrice = function () {
    return this.price;
};

Sale.decorators = {};


Sale.decorators.vat = {
    getPrice: function () {
        var price = this.super.getPrice();
        price *= 1.25;
        return price;
    }
};

Sale.decorators.het = {
    getPrice: function () {
        var price = this.super.getPrice();
        price *= 1.5;
        return price;
    }
};

var sale = new Sale(100);
var saleWithVat = sale.decorate('vat');
var saleWithVatAndHet = sale.decorate('vat').decorate('het');


console.log(sale.getPrice());                   //100
console.log(saleWithVat.getPrice());            //125
console.log(saleWithVatAndHet.getPrice());      //187.5

```



