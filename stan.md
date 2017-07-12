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




```

[https://codepen.io/Bigismall/pen/EXmXwj](https://codepen.io/Bigismall/pen/EXmXwj)

