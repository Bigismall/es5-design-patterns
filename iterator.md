# Iterator

Iterator to wzorzec oferujący prostą metodę sekwencyjnego wybierania kolejnego elementu z kolekcji.

Dane w kolekcji mogą być przechowywane wewnętrznie w bardzo złożonej strukturze, ale sekwencyjny dostęp do nich zapewnia bardzo prosta funkcja \(next\(\)\).  Kod korzystający z obiektu nie musi znać całej złożoności struktury danych - wystarczy, że wie, jak korzystać z pojedynczego elementu i pobrać następny.

```js
function randomRange(items, from, to) {
    var randomValuesArray = [], i = 0;
    for (; i < items; i++) {
        randomValuesArray.push(from + Math.floor(Math.random() * to) + 1);
    }
    return randomValuesArray;
}

var randomNumbers = randomRange(10, 0, 100),
    simpleIterator = (function (collection) {
        var pointer = 0,
            collectionLength = collection.length;

        return {
            next: function () {

                if (!this.hasNext()) {
                    return null;
                }
                return collection[pointer++];
            },
            hasNext: function () {
                return pointer < collectionLength;
            }
        }


    }(randomNumbers));


console.log(randomNumbers);         //[ 98, 94, 38, 21, 72, 67, 9, 6, 56, 88 ]

while (simpleIterator.hasNext()) {
    console.log(simpleIterator.next());
}
// 98
// 94
// 38
// 21
// 72
// 67
// 9
// 6
// 56
// 88
```



**Zastosowanie:** Pobieranie kolejnych wystąpień przedmiotu z planu lekcji

