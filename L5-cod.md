# Video, Masti si animatie de fundal

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

_Exemplu simplu:_

```js
const frame = new Frame("zim", 1000, 600, "black", ready);

// Incarcam resursa video
frame.loadAssets("natura.mp4", "video/");

function ready() {
  const stage = frame.stage;

  // Afisam video-ul
  const clipVideo = new Vid("natura.mp4", {
    loop: true, // Ruleaza in bucla
    autoplay: true, // Porneste automat
    muted: true, // Fara sunet
    width: 800,
  }).center();

  stage.update();
}
```

_Pregatirea resurselor:_

```js
// Aici vom scrie codul nostru ZIM
const frame = new Frame("zim", 1000, 600, "#222", ready);

frame.loadAssets("natura.mp4", "video/");

function ready() {
  const stage = frame.stage;

  // Cream video-ul, dar fara autoplay
  const clipVideo = new Vid("natura.mp4", {
    width: 800,
    height: 450,
  }).center();

  // -- Aici vom adauga codul pentru fiecare idee --

  stage.update();
}
```

_Mini-player:_

```js
// Adauga acest cod in interiorul lui frame.on("ready",...)

// -- Player Personalizat --
const baraProgres = new Rectangle(800, 10, "red").loc(clipVideo.x, 530);
const butonPlay = new Button({
  label: "►", // Simbolul pentru Play
  width: 80,
  height: 80,
  backgroundColor: "rgba(0,0,0,0.5)",
  rollBackgroundColor: "rgba(0,0,0,0.8)",
}).center();

butonPlay.on("click", () => {
  if (clipVideo.paused) {
    clipVideo.play();
    butonPlay.label = "❚❚"; // Simbolul pentru Pauza
  } else {
    clipVideo.pause();
    butonPlay.label = "►";
  }
  stage.update();
});

// Ascultam evenimentul 'timeupdate' care se declanseaza constant in timpul redarii
clipVideo.on("timeupdate", () => {
  // Calculam procentajul de redare
  const procent = clipVideo.currentTime / clipVideo.duration;
  // Ajustam latimea barei de progres
  baraProgres.width = 800 * procent;
  stage.update();
});
```

_Lentila magica:_

```js
// Adauga si acest cod. Asigura-te ca ai comentat sau sters codul player-ului de mai sus pentru a nu se suprapune.

// -- Lentila Magica --
// Cream un cerc care va fi masca noastra
const lentila = new Circle(150, "white").center();

// Aplicam masca! Video-ul va fi vizibil doar in interiorul formei "lentila"
clipVideo.mask(lentila);
clipVideo.play(); // Dam play sa ruleze

// Facem "lentila" sa urmareasca mouse-ul pe scena
stage.on("stagemousemove", () => {
  lentila.loc(stage.mouseX, stage.mouseY);
  stage.update();
});
```

_Fundal Animat:_

```js
// Adauga acest cod in locul celorlalte idei.

// -- Fundal Animat --
const videoFundal = new Vid("natura.mp4", {
  width: frame.width, // Ocupa toata latimea scenei
  loop: true,
  autoplay: true,
  muted: true,
}).center();

// ADAUGAM FUNDALUL PE CEL MAI DE JOS STRAT
// Orice adaugam dupa el va aparea deasupra
stage.addChildAt(videoFundal, 0);

// Adaugam un titlu deasupra video-ului
new Label({
  text: "EXPLOREAZA NATURA",
  size: 60,
  color: "white",
  bold: true,
  italic: true,
}).center();
```
