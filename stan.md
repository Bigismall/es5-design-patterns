# Stan

Umożliwia obiektowi modyfikację zachowania w wyniku zmiany wewnętrznego stanu. Wygląda to tak, jakby obiekt zmienił klasę.

```js
 class NaiveBanking{
        state = "";
        balance = 0;
        public NextState(action: string, amount: number)
        {
            if(this.state == "przekroczone" && action == "wypłata")
            {
                this.state = "wstrzymane";
            }
            if(this.state == "wstrzymane" && action != "depozyt")
            {
                this.state = "wstrzymane";
            }
            if(this.state == "dobra sytuacja" && action == "wypłata" && amount <= this.balance)
            {
                this.balance -= amount;
            }
            if(this.state == "dobra sytuacja" && action == "wypłata" && amount > this.balance)
            {
                this.balance -= amount;
                this.state = "przekroczone";
            }
        }
    }
```

Zamiast stosowania ogromnego pojedynczego bloku instrukcji `if` możemy skorzystać z wzorca Stan. Cechuje się on tym, że zawiera menadżera stanów, który dokonuje abstrakcji stanu wewnętrznego, a ponadto pośredniczy w przekazaniu komunikatu do odpowiedniego stanu zaimplementowanego jako klasa. Cała logika w obrębie stanów i zarządzanie przejściami stanów nadzorowane jest przez poszczególne klasy stanów.

```js

class IState {
    constructor() {
        this.currentState = null;
    }

    add(amount) {
        throw new ReferenceError("Unimplemented method");
    }

    remove(amount) {
        throw new ReferenceError("Unimplemented method");
    }

    getCurrentState() {
        return this.currentState.constructor.name;
    }
}

class BankAccountManager extends IState {

    constructor() {
        super();
        this.balance = 0;
        this.currentState = new GoodStandingState(this);
    }

    add(amount) {
        this.currentState.add(amount);
    }

    remove(amount) {
        this.currentState.remove(amount);
    }

    addToBalance(amount) {
        this.balance += amount;
    }

    getBalance() {
        return this.balance;
    }

    moveToState(newState) {
        this.currentState = newState;
    }
}

class GoodStandingState extends IState {
    constructor(manager) {
        super();
        this.manager = manager;
    }

    add(amount) {
        this.manager.addToBalance(amount);
    }

    remove(amount) {
        if (this.manager.getBalance() < amount) {
            this.manager.moveToState(new OverdrawnState(this.manager));
        }

        this.manager.addToBalance(-1 * amount)
    }
}

class OverdrawnState extends IState {
    constructor(manager) {
        super();
        this.manager = manager;
    }

    add(amount) {
        this.manager.addToBalance(amount);
        if (this.manager.getBalance() > 0) {
            this.manager.moveToState(new GoodStandingState(this.manager));
        }
    }

    remove(amount) {
        this.manager.moveToState(new OnHold(this.manager));
        console.log("Nie można wypłacić pieniędzy z konta bankowego z już przekroczonym saldem.");
    }
}

class OnHold extends IState {
    constructor(manager) {
        super();
        this.manager = manager;
    }

    add(amount) {
        this.manager.addToBalance(amount);
        throw new Error("Używane konto zostało zablokowane i konieczna jest wizyta w banku w celu wyjaśnienia tej kwestii.");
    }

    remove(amount) {
        throw new Error("Używane konto zostało zablokowane i konieczna jest wizyta w banku w celu wyjaśnienia tej kwestii.");
    }
}


let bankManager = new BankAccountManager();
```

[https://codepen.io/Bigismall/pen/EXmXwj](https://codepen.io/Bigismall/pen/EXmXwj)



