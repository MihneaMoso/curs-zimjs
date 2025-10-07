# Sunete in Zim

HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>ZIM - Salut Lume</title>

    <script type="module" src="https://zimjs.org/cdn/018/zim.js"></script>

    <style>
      body {
        margin: 0;
        background: #333;
      }
      #zim {
        display: block;
        background: #000;
      }
    </style>
  </head>
  <body>
    <div id="zim"></div>

    <script type="module">
      // Aici vom scrie codul nostru ZIM
      new frame("zim", 800, 600, "#333", "#999", ready);
    </script>
  </body>
</html>
```

```javascript
const frame = new Frame("zim", 800, 600, "white", "lightgray", ready);

// 1. Lista cu numele fisierelor
const assets = ["nota1.mp3", "efect.wav"];
// 2. Calea catre folderul cu sunete
const path = "sunete/";

// 3. Spunem ZIM sa preincarce aceste resurse
frame.loadAssets(assets, path);

function ready() {
  // Codul nostru va rula doar dupa ce toate resursele sunt incarcate
  const stage = frame.stage;

  // ... aici vom putea reda sunetele instantaneu

  stage.update();
}
```

```javascript
// Acest cod vine in interiorul lui frame.on("ready", ...)
// Presupunem ca am preincarcat "efect.wav"

const butonSunet = new Button({
  label: "Apasa-ma!",
  width: 200,
  height: 60,
}).center();

// Atasam un eveniment de tip "mousedown" (se declanseaza cand apasam click)
butonSunet.on("mousedown", () => {
  // Redam sunetul
  new Aud("efect.wav").play();
});
```

_Cod proiect claviatura_:

```javascript
// Aici vom scrie codul nostru ZIM
const frame = new Frame("zim", 800, 600, "black", "darkgray", ready);

// Definim notele si le preincarcam
const note = ["do", "re", "mi", "fa", "sol", "la", "si"];
const assets = [];
for (let i = 0; i < note.length; i++) {
  assets.push(note[i] + ".mp3");
}
frame.loadAssets(assets, "sunete/");

function ready() {
  const stage = frame.stage;

  new Label("Pianul ZIM").center().mov(0, -250);

  const latimeClapa = 80;
  const inaltimeClapa = 300;
  const spatiere = 10;
  const xInitial = 100;

  // Folosim o bucla 'loop' pentru a crea cele 7 clape albe
  loop(note.length, (i) => {
    // Cream un dreptunghi alb pentru clapa
    const clapa = new Rectangle({
      width: latimeClapa,
      height: inaltimeClapa,
      color: "white",
      borderColor: "black",
      borderWidth: 2,
    });

    // Calculam pozitia fiecarei clape
    clapa.loc(xInitial + i * (latimeClapa + spatiere), 150);

    // Aici este magia!
    // Atasam un eveniment 'mousedown' fiecarei clape
    clapa.on("mousedown", () => {
      // Redam sunetul corespunzator clapei
      // note[i] va fi "do", "re", etc., in functie de clapa
      new Aud(note[i] + ".mp3").play();

      // Efect vizual: schimbam culoarea la apasare
      clapa.color = "lightgray";
      stage.update();
    });

    // Bonus: revenim la culoarea initiala cand luam click-ul de pe clapa
    clapa.on("pressup", () => {
      clapa.color = "white";
      stage.update();
    });
  });

  stage.update();
}
```
