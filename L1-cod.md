# Intro ZIM.js

_Index.html:_

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

_script tag:_

```html
<script type="module">
  // Aici vom scrie codul nostru ZIM
  const frame = new Frame("zim", 800, 600, "#333", "#999", ready);

  function ready() {
    const stage = frame.stage;

    new Circle(100, "orange").center().drag(); // Il face sa poata fi tras cu mouse-ul

    stage.update();
  }
</script>
```
