# Fasada

Wzorzec fasady jest bardzo prosty i ma za zadanie zapewnić alternatywny \(uproszczony\) interfejs obiektu.

Dobrą praktyka jest stosowanie krótkich metod, które nie wykonują zbyt wielu zadań. Stosując to podejście, uzyskuje się znacznie więcej metod niż w przypadku tworzenie **supermetod** z wieloma parametrami. W  większości sytuacji dwie lub więcej metod wykonuje się jednocześnie. Warto wtedy utworzyć jeszcze jedną metodę, który stanowi otoczę dla takich połączeń.

| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |

W trakcie obsługi zdarzeń przeglądarki bardzo często korzysta się z następujących metod:

* `stopPropagation()` - zapobiega wykonywaniu obsługi zdarzenia w węzłach nadrzędnych
* `preventDefault()` - zapobiega wykonaniu przez przeglądarkę domyślnej akcji dla zdarzenia

To dwie osobne metody wykonujące odmienne zadania,  więc nie stanowią jednej całości, ale z drugiej strony w zdecydowanej większości sytuacji są one wykonywane jednocześnie. Zamiast więc powielać wywołania obu metod w całej aplikacji, można utworzyć fasadę, która je obie wykona.

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



