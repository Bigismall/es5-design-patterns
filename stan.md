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

Zamiast stosowania ogromnego pojedynczego bloku instrukcji `if` możemy skorzystać z wzorca Stan. Cechuje się on tym, że zawiera menadżera stanów



