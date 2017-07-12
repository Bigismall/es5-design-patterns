# Mediator

Wzorzec mediatora ma za zadanie promować **luźne powiązania** obiektów i wspomóc przyszłą konserwację kodu. W tym wzorcu niezależne obiekty nie komunikują się ze sobą bezpośrednio, ale korzystają z obiektu mediatora. Gdy jeden z kolegów zmieni stan, informuje o tym mediator, a ten przekazuje tę informację wszystkim innym zainteresowanym.

```js
function Player(name, gameMediator) {
    this.name = name;
    this.points = 0;
    this.gameMediator = gameMediator;

    this.play = function () {
        this.points++;
        this.gameMediator.played();
    };
}


function ScoreBoard() {
    this.board = [];

    this.update = function (players) {
        this.board = [];
        for (var p in players) {
            if (players.hasOwnProperty(p)) {
                this.board.push({
                    name: players[p].name,
                    point: players[p].points
                });
            }
        }
        this.showBoard();
    };

    this.showBoard = function () {
        console.log("----------- SCORE BOARD -----------");
        this.board.forEach(function (b) {
            return console.log(b.name + ": " + b.point);
        });
    };
}


function GameMediator() {
    this.players = {};
    this.players["red"] = new Player("Red", this);
    this.players["blue"] = new Player("Blue", this);
    this.players["yellow"] = new Player("Yellow", this);
    this.scoreBoard = new ScoreBoard();

    this.played = function () {
        this.scoreBoard.update(this.players);
    };
    this.keyPress = function (e) {
        if (e.code === "KeyR") {
            this.players.red.play();
        }
        if (e.code === "KeyB") {
            this.players.blue.play();
        }
        if (e.code === "KeyY") {
            this.players.yellow.play();
        }
    };
}

var gameMediator = new GameMediator();

window.addEventListener("keypress", function (e) {
    gameMediator.keyPress(e);
});

```

[https://codepen.io/Bigismall/pen/ZywEQp](https://codepen.io/Bigismall/pen/ZywEQp)

