# Fasada

Wzorzec fasady jest bardzo prosty i ma za zadanie zapewnić alternatywny \(uproszczony\) interfejs obiektu.

Fasada to w zasadzie specjalny przypadek wzorca Adapter, który zapewnia uproszczony interfejs dla **kolekcji **klas.

| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |

```js
var myevent = {

    stop: function (e) {

// others
        if (typeof e.preventDefault === "function") {
            e.preventDefault();
        }
        if (typeof e.stopPropagation === "function") {
            e.stopPropagation();
        }
// IE
        if (typeof e.returnValue === "boolean") {
            e.returnValue = false;
        }
        if (typeof e.cancelBubble === "boolean") {
            e.cancelBubble = true;
        }
    }
// ...
};
```



