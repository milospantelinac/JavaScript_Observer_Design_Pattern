# JavaScript - Observer Design Pattern

Dizajn patern Observer je iz bihevioralne kategorije i nudi model pretplate (subscription) u kojem se objekti pretplaćuju (subscribe) na event-e i dobijaju obaveštenja kada se desi event. Ovaj patern je kamen temeljac za programiranje event-a, uključujući JavaScript. Patern Observer olakšava dobar objektno orijentisani dizajn i potstiče loose coupling kod.

## Upotreba Observer-a

Prilikom izrade web aplikacije pisaćete mnogo **evenet handler-a**. Event handleri su funkcije koje će biti obaveštene/pozvane kada se aktivira određeni event. Ova obaveštenja opcionalno primaju argument event s pojedinostima o event-u (na primer položaj x i y miša pri kliku).

Paradigma evenet handlers-a i event-a u JavaScriptu je manifestacija dizajn paterna Observer. Drugi naziv za patern Observer je **Pub/Sub**, skraćeno od **Publication/Subscription**.

## Diagram

<img width="300" alt="observer_patern" src="https://user-images.githubusercontent.com/21141150/206925042-4cf2a6f2-b3bc-4d11-9c7c-673febfc0947.png">

## Učesnici

Objekti koji učestvuju u ovom paternu su:

Subject -- U primeru: Click
1. Vodi lisu observers-a. Subjekt može da ima bilo koji broj Observer objekata.
2. Implementira interfejs koji Observer objektima omogućuje pretplatu ili odjavu.
3. Šalje obaveštenja svojim Observerima kada se njegovo stanje promeni.

Observers -- U primeru: clickHandler
1. Ima potpis funkcije koji se može pozvati kada se Subjekt promeni (tj. desi se događaj)


## Primer

Objekat Click predstavlja Subjekt. Funkcija clickHandler je Observer koji se pretplaćuje. Ovaj subscribes handler se pretplaćuje, odjavljuje, a zatim se pretplaćuje dok se event-i pokreću. Dobija obaveštenje samo o događajima #1 i #3.

Primetite da metoda **fire** prihvata dva argumenta. Prvi sadrži detalje o event-u, a drugi je kontekst, odnosno vrednost **this** kada se pozivaju event handlers. Ako nije naveden kontekst, ovo će biti vezano za globalni objekt (window).

```
function Click() {
    this.handlers = [];  // observers
}

Click.prototype = {

    subscribe: function (fn) {
        this.handlers.push(fn);
    },

    unsubscribe: function (fn) {
        this.handlers = this.handlers.filter(
            function (item) {
                if (item !== fn) {
                    return item;
                }
            }
        );
    },

    fire: function (o, thisObj) {
        var scope = thisObj || window;
        this.handlers.forEach(function (item) {
            item.call(scope, o);
        });
    }
}

function run() {

    var clickHandler = function (item) {
        console.log("fired: " + item);
    };

    var click = new Click();

    click.subscribe(clickHandler);
    click.fire('event #1');
    click.unsubscribe(clickHandler);
    click.fire('event #2');
    click.subscribe(clickHandler);
    click.fire('event #3');
}
// → fired: event #1
// → fired: event #3
```
