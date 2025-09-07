# Crearea Formelor Proprii cu CustomShape

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

```js
// Aici vom scrie codul nostru ZIM
const frame = new Frame("fit", 1024, 768, "white", "lightgray", ready);

function ready() {
  const stage = frame.stage;

  // -- DEFINIRE CUSTOM SHAPE: STEA --
  const Stea = function (color) {
    this.super_constructor();
    this._color = color;
    this.shape = new Shape().addTo(this);
    this.colorCommand = this.shape.f(this._color).command;

    // Desenam o stea cu 5 colturi
    this.shape.mt(0, -50);
    for (let i = 0; i < 5; i++) {
      this.shape.lt(
        Math.cos(((18 + i * 72) / 180) * Math.PI) * 50,
        -Math.sin(((18 + i * 72) / 180) * Math.PI) * 50
      );
      this.shape.lt(
        Math.cos(((54 + i * 72) / 180) * Math.PI) * 20,
        -Math.sin(((54 + i * 72) / 180) * Math.PI) * 20
      );
    }
  };
  extend(Stea, CustomShape);

  // -- DEFINIRE CUSTOM SHAPE: INIMA --
  const Inima = function (color) {
    this.super_constructor();
    this._color = color;
    this.shape = new Shape().addTo(this);
    this.colorCommand = this.shape.f(this._color).command;

    // Desenam o inima folosind curbe Bezier
    this.shape
      .mt(0, -15)
      .bt(-40, -60, -80, 0, 0, 40)
      .bt(80, 0, 40, -60, 0, -15);
  };
  extend(Inima, CustomShape);

  // -- Aici va urma codul pentru interfata si logica aplicatiei --

  stage.update();
}
```

```js
// Adauga acest cod in continuarea celui de mai sus, in interiorul lui function ready() {...}

// -- INTERFATA & LOGICA PAINT --
let formaSelectata = "stea";
let culoareSelectata = "red";

// Pânza de desen
const panza = new Rectangle(800, 700, "white", "black", 1).loc(200, 50);

// Panoul de control
new Rectangle(180, 700, "lightgray").loc(10, 50);
new Label("FORME").pos(100, 80);

// Butoanele pentru selectia formelor
const btnStea = new Stea("black").pos(100, 140).sca(0.7);
const btnInima = new Inima("black").pos(100, 220).sca(0.5);

btnStea.on("click", () => (formaSelectata = "stea"));
btnInima.on("click", () => (formaSelectata = "inima"));

// Selectorul de culoare
new Label("CULORI").pos(100, 300);
const selectorCuloare = new ColorPicker({
  color: culoareSelectata,
  width: 140,
}).pos(30, 330);

selectorCuloare.on("change", () => {
  culoareSelectata = selectorCuloare.color;
});

// Logica de desenare pe panza
panza.on("mousedown", () => {
  let formaNoua;
  if (formaSelectata === "stea") {
    formaNoua = new Stea(culoareSelectata);
  } else {
    // Inima
    formaNoua = new Inima(culoareSelectata);
  }

  // Pozitionam forma unde a dat click utilizatorul
  // frame.mouseX/Y ne da pozitia globala, trebuie sa o ajustam la pozitia panzei
  formaNoua.loc(frame.mouseX - panza.x, frame.mouseY - panza.y).addTo(panza);

  stage.update();
});
```
