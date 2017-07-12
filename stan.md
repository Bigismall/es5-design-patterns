# Stan

Umożliwia obiektowi modyfikację zachowania w wyniku zmiany wewnętrznego stanu. Wygląda to tak, jakby obiekt zmienił klasę.

```js
function NaiveBanking() {
    this.state = "";
    this.balance = 0;

    this.nextState = function (action, amount) {
        if (this.state == "przekroczone" && action == "wypłata") {
            this.state = "wstrzymane";
        }
        if (this.state == "wstrzymane" && action != "depozyt") {
            this.state = "wstrzymane";
        }
        if (this.state == "dobra sytuacja" && action == "wypłata" && amount <= this.balance) {
            this.balance -= amount;
        }
        if (this.state == "dobra sytuacja" && action == "wypłata" && amount > this.balance) {
            this.balance -= amount;
            this.state = "przekroczone";
        }
    }
}
```

Zamiast stosowania ogromnego pojedynczego bloku instrukcji `if` możemy skorzystać z wzorca Stan. Cechuje się on tym, że zawiera menadżera stanów, który dokonuje abstrakcji stanu wewnętrznego, a ponadto pośredniczy w przekazaniu komunikatu do odpowiedniego stanu zaimplementowanego jako klasa. Cała logika w obrębie stanów i zarządzanie przejściami stanów nadzorowane jest przez poszczególne klasy stanów.

```js
function IState() {
    this.currentState = null;

    this.add = function (amount) {
        throw new ReferenceError("Unimplemented method");
    };

    this.subtract = function (amount) {
        throw new ReferenceError("Unimplemented method");
    };

    this.getCurrentState = function () {
        return this.currentState.constructor.name;
    };
}


function BankAccountManager() {

    IState.call(this);

    this.balance = 0;
    this.currentState = new GoodStandingState(this);


    this.add = function (amount) {
        this.currentState.add(amount);
    };

    this.subtract = function (amount) {
        this.currentState.subtract(amount);
    };

    this.addToBalance = function (amount) {
        this.balance += amount;
    };

    this.getBalance = function () {
        return this.balance;
    };

    this.moveToState = function (newState) {
        this.currentState = newState;
    }
}

BankAccountManager.prototype = Object.create(IState.prototype);
BankAccountManager.prototype.constructor = BankAccountManager;


function GoodStandingState(manager) {

    IState.call(this);
    this.manager = manager;

    this.add = function (amount) {
        this.manager.addToBalance(amount);
    };

    this.subtract = function (amount) {
        if (this.manager.getBalance() < amount) {
            this.manager.moveToState(new OverdrawnState(this.manager));
        }

        this.manager.addToBalance(-1 * amount)
    };
}

GoodStandingState.prototype = Object.create(IState.prototype);
GoodStandingState.prototype.constructor = GoodStandingState;


function OverdrawnState(manager) {

    IState.call(this);
    this.manager = manager;

    this.add = function (amount) {
        this.manager.addToBalance(amount);
        if (this.manager.getBalance() > 0) {
            this.manager.moveToState(new GoodStandingState(this.manager));
        }
    };

    this.subtract = function (amount) {
        this.manager.moveToState(new OnHoldState(this.manager));
    };
}

OverdrawnState.prototype = Object.create(IState.prototype);
OverdrawnState.prototype.constructor = OverdrawnState;

function OnHoldState(manager) {

    IState.call(this);
    this.manager = manager;
    console.log("Nie można wypłacić pieniędzy z konta bankowego z już przekroczonym saldem.");


    this.add = function (amount) {
        this.manager.addToBalance(amount);
        throw new Error("Używane konto zostało zablokowane i konieczna jest wizyta w banku w celu wyjaśnienia tej kwestii.");
    }

    this.subtract = function (amount) {
        throw new Error("Używane konto zostało zablokowane i konieczna jest wizyta w banku w celu wyjaśnienia tej kwestii.");
    }
}

OnHoldState.prototype = Object.create(IState.prototype);
OnHoldState.prototype.constructor = OnHoldState;


var bankManager = new BankAccountManager();
try {

    console.log(bankManager.getBalance(), bankManager.getCurrentState());
    bankManager.add(500);
    console.log(500, bankManager.getBalance(), bankManager.getCurrentState());
    bankManager.subtract(400);
    console.log(-400, bankManager.getBalance(), bankManager.getCurrentState());
    bankManager.subtract(200);
    console.log(-200, bankManager.getBalance(), bankManager.getCurrentState());
    bankManager.subtract(200);
    console.log(-200, bankManager.getBalance(), bankManager.getCurrentState());
    bankManager.add(1000);
    console.log(1000, bankManager.getBalance(), bankManager.getCurrentState());

} catch (e) {
    console.log('ERROR: ' + e.message);
}




```



