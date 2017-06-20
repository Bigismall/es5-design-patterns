# Mediator

Wzorzec mediatora ma za zadanie promować **luźne powiązania** obiektów i wspomóc przyszłą konserwację kodu. W tym wzorcu niezależne obiekty nie komunikują się ze sobą bezpośrednio, ale korzystają z obiektu mediatora. Gdy jeden z kolegów zmieni stan, informuje o tym mediator, a ten przekazuje tę informację wszystkim innym zainteresowanym.

```js
class HQ {
    constructor() {
        this.landForces = new LandForces(this);
        this.seaForces = new SeaForces(this);
        this.airForces = new AirForces(this);
        this.specialForces = new SpecialForces(this);
    }

    routeMessage(message, important = false) {
        this.landForces.receiveMessage(message);
        this.seaForces.receiveMessage(message);
        this.airForces.receiveMessage(message);

        //tu może być złożona logika
        if (important) {
            this.specialForces.receiveMessage(message);
        }
    }
}

class LandForces {
    constructor(headQuarters) {
        this.headQuarters = headQuarters;
    }

    receiveMessage(message) {
        console.log(`Land forces received message: ${message}`);
    }

    sendMessage(message) {
        this.headQuarters.routeMessage(message);
    }
}

....
```

[https://codepen.io/Bigismall/pen/owWwPW](https://codepen.io/Bigismall/pen/owWwPW)

[https://codepen.io/Bigismall/pen/xrgXqK](https://codepen.io/Bigismall/pen/xrgXqK)

