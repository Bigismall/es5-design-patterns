# Iterator

sadsadsa





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



