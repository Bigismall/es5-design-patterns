# Fasada

Wzorzec fasady jest bardzo prosty i ma za zadanie zapewnić alternatywny \(uproszczony\) interfejs obiektu.

Fasada to w zasadzie specjalny przypadek wzorca Adapter, który zapewnia uproszczony interfejs dla **kolekcji **klas.


| Wzorzec | Przeznaczenie |
| :--- | :--- |
| Adapter | Dokonuje konwersji jednego interfejsu na inny |
| Dekorator | Nie modyfikuje interfejsu, ale dodaje zachowania |
| Fasada | Powoduje uproszczenie interfejsu |



```js
'use strict';

const TYPE_LAND = 0;
const TYPE_FLAT = 1;
const TYPE_HOUSE = 2;

class Mortgage {

    constructor(name, type) {
        this.name = name;
        this.type = type;
        this.result = false;
    }

    applyFor(amount) {
        this.amount = amount;

        if (!Bank.verify(this.name, this.amount)) {
            this.result = false;
            this.message = 'Bank';

        } else if (!Credit.get(this.name, this.type)) {
            this.result = false;
            this.message = 'Credit';
        } else {
            let operand = new Operand(this.amount, this.type);
            if (!operand.check()) {
                this.result = false;
                this.message = 'Operand';
            } else {
                this.result = true;
            }
        }

        return this.result;
    }
}

class Bank {
    static verify(name, amount) {
        return (name.endsWith('ski') && amount <= 1000000);
    }
}

class Credit {
    static get(name, type) {
        let result = false;
        switch (type) {
            case TYPE_LAND :
                result = !name.startsWith('Janusz');
                break;
            case TYPE_FLAT :
                result = name.endsWith('ski');      //tylko prawdziwi polacy
                break;
            case TYPE_HOUSE :
                result = !name.endsWith('a');       //bez kobiet
                break;
            default:
                result = false;
                break;
        }
        return result;
    }
}


class Operand {
    constructor(amount, type) {
        this.amount = amount;
        this.type = type;
    }

    check() {
        let result;
        switch (this.type) {
            case TYPE_LAND :
                result = (this.amount <= 250000);
                break;
            case TYPE_FLAT :
                result = (this.amount> 250000 && this.amount <= 500000);
                break;
            case TYPE_HOUSE :
                result = (this.amount >500000 && this.amount <= 1000000);
                break;
            default:
                result = false;
                break;
        }
        return result;
    }
}
```

[https://codepen.io/Bigismall/pen/Ogmpyd](https://codepen.io/Bigismall/pen/Ogmpyd)

