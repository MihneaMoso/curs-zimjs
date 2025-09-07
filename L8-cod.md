# Tamagotchi

HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>ZIM - Salut Lume</title>

    <script src="https://zimjs.org/cdn/018/zim.js"></script>

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
const frame = new Frame("fit", 800, 600, "lightgreen", "green", ready);

// Optional: Incarcam sunete pentru interactivitate
frame.loadAssets(["eat.mp3", "drink.mp3", "pet.mp3"], "sunete/");

function ready() {
  const stage = frame.stage;

  // -- 1. VARIABILELE JOCULUI (STAREA) --
  let foame = 100;
  let sete = 100;
  let fericire = 100;
  let stareAnimal = "neutru";

  // -- 2. CREAREA ANIMALUTULUI (ZIMAGOTCHI) --
  const animal = new Container(200, 220).center().mov(0, -50);

  // Corpul
  new Circle(100, "yellow", "black", 2).addTo(animal);

  // Ochii
  new Circle(15, "white", "black", 2).loc(-40, -20, animal);
  new Circle(15, "white", "black", 2).loc(40, -20, animal);
  new Circle(5, "black").loc(-40, -20, animal);
  new Circle(5, "black").loc(40, -20, animal);

  // Gura (o vom modifica in functie de stare)
  const gura = new Shape().loc(0, 30, animal);

  const seteazaExpresie = (expresie) => {
    gura.graphics.clear(); // Stergem desenul anterior al gurii
    if (expresie === "fericit") {
      gura.graphics.f("black").arc(0, 0, 40, 0, 180); // Un zambet larg
    } else if (expresie === "trist") {
      gura.graphics.f("black").arc(0, 20, 40, 180, 360); // O gura incruntata
    } else {
      // Neutru
      gura.graphics.f("black").ss(5).mt(-20, 0).lt(20, 0); // O linie dreapta
    }
    stage.update();
  };
  seteazaExpresie("neutru");

  // -- 3. INTERFATA: BARE DE STATUS SI BUTOANE --
  new Label("NEVOILE ZIMAGOTCHI-ULUI TAU:", 20).pos(frame.center.x, 380);

  // Indicatoarele pentru nevoi
  const baraFoame = new Indicator({ label: "Foame", width: 200 }).pos(
    frame.center.x,
    420
  );
  const baraSete = new Indicator({ label: "Sete", width: 200 }).pos(
    frame.center.x,
    460
  );
  const baraFericire = new Indicator({ label: "Fericire", width: 200 }).pos(
    frame.center.x,
    500
  );

  // Butoanele de actiune (stilate ca in Lectia 6)
  const btnHraneste = new Button({
    label: "Hrănește",
    width: 150,
    corner: 20,
  }).pos(100, 550);
  const btnAdapa = new Button({ label: "Adapă", width: 150, corner: 20 }).pos(
    frame.center.x,
    550
  );
  const btnMangaie = new Button({
    label: "Mângâie",
    width: 150,
    corner: 20,
  }).pos(frame.width - 100, 550);

  // -- 4. LOGICA JOCULUI --

  // Actiunile butoanelor
  btnHraneste.on("click", () => {
    foame = Math.min(100, foame + 25); // Creste foamea, dar nu peste 100
    sete = Math.max(0, sete - 5); // Cand mananca, i se face putin sete
    zim.playSound("eat.mp3");
  });
  btnAdapa.on("click", () => {
    sete = Math.min(100, sete + 30);
    zim.playSound("drink.mp3");
  });
  btnMangaie.on("click", () => {
    fericire = Math.min(100, fericire + 20);
    zim.playSound("pet.mp3");
    animal.animate({ props: { rotation: -10 }, time: 100, rewind: true }); // Efect de bucurie
  });

  // Scaderea nevoilor in timp
  interval(2000, () => {
    foame = Math.max(0, foame - 3);
    sete = Math.max(0, sete - 4);
    fericire = Math.max(0, fericire - 2);
  });

  // Bucla principala a jocului (Ticker)
  Ticker.add(() => {
    // Actualizam barele de progres
    baraFoame.percent = foame;
    baraSete.percent = sete;
    baraFericire.percent = fericire;

    // Verificam starea si actualizam expresia
    let nouaStare = "neutru";
    if (foame <= 0 || sete <= 0 || fericire <= 0) {
      nouaStare = "trist";
    } else if (foame > 80 && sete > 80 && fericire > 80) {
      nouaStare = "fericit";
    }

    if (nouaStare !== stareAnimal) {
      stareAnimal = nouaStare;
      seteazaExpresie(stareAnimal);
    }
  });

  stage.update();
}
```
