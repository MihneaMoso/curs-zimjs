# Frame-uri Responsive și Butoane Avansate

HTML:
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<title>ZIM - Salut Lume</title>

<script src="https://zimjs.org/cdn/018/zim.js"></script>

<style>
    body {margin: 0; background: #333;}
    #zim {display: block; background: #000;}
</style>

</head>
<body>

<div id="zim"></div>

<script type="module">
// Aici vom scrie codul nostru ZIM
new Frame("zim", 800, 600, "#333", "#999");
</script>

</body>
</html>
```

```js
// In loc de "zim", folosim "fit" ca prim parametru.
// Dimensiunile 1024x768 devin proportia noastra de baza.
const frame = new Frame("fit", 1024, 768, "#EEE", "#AAA");
```

```js
// -- Buton standard --
new Button({label: "Click"}).loc(100, 100);

// -- Buton avansat, personalizat --
new Button({
    label: "Start",
    width: 200,
    height: 70,
    backgroundColor: "green",
    rollBackgroundColor: "darkgreen",
    corner: 35, // Foarte rotunjit
    borderColor: "lightgreen",
    borderWidth: 4,
    icon: new Triangle(null, null, null, "white").rot(-90) // O iconita de "play"
}).loc(100, 200);
```

*Zim Runner:*
```js
// Aici vom scrie codul nostru ZIM
const frame = new Frame("fit", 800, 600, "lightblue", "darkblue");

frame.on("ready", () => {
    const stage = frame.stage;
    let stageW = frame.width;
    let stageH = frame.height;

    // -- SETUP JOC --
    const culoare = [-200, 0, 200]; // Coordonatele X pentru cele 3 culoare
    let indexCuloar = 1; // Incepem pe culoarul din mijloc
    let jucator, scorLabel, scor = 0;
    let obstacole = new Container();
    let jocActiv = true;

    // -- JUCATOR --
    jucator = new Rectangle(80, 150, "orange").center().mov(0, 150);
    jucator.stare = "alearga"; // Stari posibile: alearga, sare, se ghemuie

    // -- MISCARE JUCATOR --
    const mutaStanga = () => { if (indexCuloar > 0) indexCuloar--; actualizeazaPozitia(); };
    const mutaDreapta = () => { if (indexCuloar < 2) indexCuloar++; actualizeazaPozitia(); };
    const actualizeazaPozitia = () => jucator.animate({x: frame.center.x + culoare[indexCuloar]}, 200);

    const sari = () => {
        if (jucator.stare !== "alearga") return;
        jucator.stare = "sare";
        jucator.animate({y: jucator.y - 200}, 300, "quadOut", () => {
            jucator.animate({y: jucator.y + 200}, 300, "quadIn", () => jucator.stare = "alearga");
        });
    };
    const ghemuire = () => {
        if (jucator.stare !== "alearga") return;
        jucator.stare = "se ghemuie";
        jucator.height = 75;
        jucator.y += 75;
        timeout(500, () => {
            jucator.height = 150;
            jucator.y -= 75;
            jucator.stare = "alearga";
        });
    };

    // -- CONTROALE --
    // Tastatura
    frame.on("keydown", (e) => {
        if (!jocActiv) return;
        if (e.key === "ArrowLeft") mutaStanga();
        if (e.key === "ArrowRight") mutaDreapta();
        if (e.key === "ArrowUp") sari();
        if (e.key === "ArrowDown") ghemuire();
    });
    // Butoane pe ecran
    const btnStanga = new Button({label:"<", width:80, height:80, corner:40}).loc(50, stageH - 100).on("mousedown", mutaStanga);
    const btnDreapta = new Button({label:">", width:80, height:80, corner:40}).loc(stageW - 130, stageH - 100).on("mousedown", mutaDreapta);
    const btnSus = new Button({label:"▲", width:80, height:80, corner:40}).loc(stageW - 130, stageH - 200).on("mousedown", sari);
    const btnJos = new Button({label:"▼", width:80, height:80, corner:40}).loc(stageW - 130, stageH - 100).on("mousedown", ghemuire);
    new Tiling([btnStanga, btnDreapta, btnSus, btnJos], 2, 2, 10, 10).loc(50, stageH-210);

    // -- OBSTACOLE & LOGICA JOC --
    obstacole.addTo(stage);
    interval(1500, () => {
        if (!jocActiv) return;
        const tipObstacol = rand() > 0.5 ? "jos" : "sus";
        const obstacol = new Rectangle(100, (tipObstacol === "jos" ? 80 : 200), "red");
        obstacol.tip = tipObstacol;
        obstacol.loc(frame.center.x + culoare[rand(2)], -200).addTo(obstacole);
        obstacol.animate({y: stageH + 200}, 2000, "linear", () => obstacol.removeFrom(obstacole));
    });

    // -- SCOR & COLIZIUNI --
    scorLabel = new Label("Scor: 0", 30).loc(50, 50);
    Ticker.add(() => {
        if (!jocActiv) return;
        scor++;
        scorLabel.text = "Scor: " + scor;
        obstacole.loop(obstacol => {
            if (obstacol.hitTestBounds(jucator)) {
                if (obstacol.tip === "jos" && jucator.stare !== "sare") gameOver();
                if (obstacol.tip === "sus" && jucator.stare !== "se ghemuie") gameOver();
            }
        });
    });
    
    const gameOver = () => {
        jocActiv = false;
        new Label("GAME OVER", 80).center();
        stage.update();
    };

    stage.update();
});
```