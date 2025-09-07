# Obiecte Vizuale si Proprietati

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
// Aici vom scrie codul nostru ZIM
const frame = new Frame("zim", 800, 600, "#EEE", "#AAA", ready);

function ready() {
  const stage = frame.stage;

  // 1. Un dreptunghi albastru
  // new Rectangle(latime, inaltime, culoare)
  new Rectangle(200, 100, "blue");

  // 2. Un triunghi rosu
  // new Triangle(laturaA, laturaB, laturaC, culoare)
  // Daca dam doar o valoare, face un triunghi echilateral
  new Triangle(100, "red");

  // 3. Un text (eticheta)
  // new Label("textul dorit", dimensiuneFont, "font", "culoareText")
  new Label("Salut ZIM!", 40, "Arial", "black");

  stage.update(); // Important! Actualizeaza scena pentru a afisa elementele
}
```

```javascript
// Metoda clasica, cu parametri in ordine
new Rectangle(200, 100, "blue", "orange", 5); // latime, inaltime, culoare, culoareBordura, latimeBordura

// Metoda moderna, cu obiect de configurare (recomandata!)
new Rectangle({
  width: 200,
  height: 100,
  color: "blue",
  borderColor: "orange",
  borderWidth: 5,
});
```

_Cod complet:_

```javascript
// Aici vom scrie codul nostru ZIM
const frame = new Frame("zim", 800, 600, "lightgray", "gray", ready);

function ready() {
  const stage = frame.stage;

  // Un titlu centrat sus
  new Label({
    text: "LECTIA 2 - FORME SI POZITII",
    size: 40,
    color: "darkblue",
  })
    .center()
    .mov(0, -250); // Il centram, apoi il mutam 250px in sus

  // Un dreptunghi ca baza
  new Rectangle({
    width: 300,
    height: 150,
    color: "lightblue",
    borderColor: "blue",
    borderWidth: 4,
  }).loc(100, 200); // Il pozitionam la x=100, y=200

  // Un cerc scalat, deasupra dreptunghiului
  new Circle({
    radius: 50,
    color: "orange",
  })
    .loc(450, 300) // Il pozitionam in dreapta
    .sca(1.5); // Il facem de 1.5 ori mai mare

  // Un triunghi rotit si interactiv
  new Triangle({
    a: 100,
    b: 100,
    color: "green",
  })
    .loc(250, 450) // Il pozitionam jos
    .rot(45) // Il rotim cu 45 de grade
    .drag(); // Il facem sa poata fi tras cu mouse-ul

  stage.update();
}
```
