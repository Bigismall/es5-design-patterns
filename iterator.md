# Iterator

Iterator to wzorzec oferujący prostą metodę sekwencyjnego wybierania kolejnego elementu z kolekcji.

Dane w kolekcji mogą być przechowywane wewnętrznie w bardzo złożonej strukturze, ale sekwencyjny dostęp do nich zapewnia bardzo prosta funkcja \(next\(\)\).  Kod korzystający z obiektu nie musi znać całej złożoności struktury danych - wystarczy, że wie, jak korzystać z pojedynczego elementu i pobrać następny.

```js
class SolwitIterator {
    constructor(collection) {
        this.data = collection;
        this.dataLength = this.data.length;
        this.pointer = 0;
    };

    hasNext() {
        return this.pointer < this.dataLength;
    }

    next() {
        if (!this.hasNext()) {
            return null;
        }
        return this.data[this.pointer++];
    }
}


class SolwitEvenNumberIterator {
    constructor(collection) {
        this.data = collection;
        this.dataLength = this.data.length;
        this.pointer = this.getNextPointer();
    };

    getNextPointer() {
        if (undefined === this.pointer) {
            return this.data.findIndex((element) => 0 === element % 2);
        }
        return this.data.findIndex((element, dataIndex) => {
            return (dataIndex > this.pointer) && (0 === element % 2)
        });
    }

    hasNext() {
        return this.pointer !== -1;
    }

    next() {
        let element;
        if (!this.hasNext()) {
            return null;
        }

        element = this.data[this.pointer];
        this.pointer = this.getNextPointer();
        return element;
    }
}
```

[https://codepen.io/Bigismall/pen/bRWqxd](https://codepen.io/Bigismall/pen/bRWqxd)

**Zastosowanie:** Pobieranie kolejnych wystąpień przedmiotu z planu lekcji

