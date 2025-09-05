# Imagini in Zim

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

```javascript
const frame = new Frame("zim", 800, 600, "lightblue");

// Incarcam imaginea
frame.loadAssets("cat.jpg", "imagini/");

frame.on("ready", () => {
    const stage = frame.stage;

    // Afisam imaginea incarcata
    new Pic("cat.jpg")
        .center()
        .sca(0.5); // O facem 50% din marimea originala

    stage.update();
});
```

```js
// Acest cod vine in interiorul lui frame.on("ready", ...)

frame.on("keydown", (e) => {
    // Verificam ce tasta a fost apasata
    if (e.key == "ArrowLeft") {
        zog("Ai apasat sageata STANGA!");
    } else if (e.key == "ArrowRight") {
        zog("Ai apasat sageata DREAPTA!");
    }
});
```

*Proiect album foto:*
```js
// Aici vom scrie codul nostru ZIM
const frame = new Frame("zim", 1000, 600, "#333", "#555");

// 1. Definim imaginile si le preincarcam
const imagini = ["poza1.jpg", "poza2.jpg", "poza3.jpg", "poza4.jpg"];
frame.loadAssets(imagini, "imagini/");

frame.on("ready", () => {
    const stage = frame.stage;

    // 2. Variabila care tine minte la ce imagine suntem
    let indexCurent = 0;

    // 3. Cream obiectul Pic care va afisa imaginea.
    // Il punem intr-o variabila pentru a-l putea modifica mai tarziu.
    const containerImagine = new Pic(imagini[indexCurent]).center();

    // Functia care schimba imaginea afisata
    const schimbaImagine = () => {
        // Schimbam sursa imaginii din container
        containerImagine.asset = frame.asset(imagini[indexCurent]);
        stage.update();
    };

    // 4. Cream butoanele de navigare
    const btnInapoi = new Button({label: "<", width: 80}).loc(50, 260);
    const btnInainte = new Button({label: ">", width: 80}).loc(870, 260);

    // 5. Adaugam logica pentru butoane
    btnInapoi.on("click", () => {
        if (indexCurent > 0) {
            indexCurent--;
            schimbaImagine();
        }
    });

    btnInainte.on("click", () => {
        if (indexCurent < imagini.length - 1) {
            indexCurent++;
            schimbaImagine();
        }
    });

    // 6. Adaugam logica pentru navigarea de la tastatura
    frame.on("keydown", (e) => {
        if (e.key == "ArrowLeft") {
            if (indexCurent > 0) {
                indexCurent--;
                schimbaImagine();
            }
        } else if (e.key == "ArrowRight") {
            if (indexCurent < imagini.length - 1) {
                indexCurent++;
                schimbaImagine();
            }
        }
    });

    stage.update();
});
```