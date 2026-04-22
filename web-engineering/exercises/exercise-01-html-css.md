---
course: web-engineering
type: exercise
date: 2026-04-09
tags:
  - web-engineering
  - exercise
  - html
  - css
  - bootstrap
source: p01-NDSS
---

# Exercise 1 — HTML & CSS Landingpage

**Repo:** `p01-NDSS`
**Dateien:** `index.html`, `styles/landingpage.css`

## index.html

```html

<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css"
          rel="stylesheet"
          integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB"
          crossorigin="anonymous">
    <link rel="stylesheet" href="styles/landingpage.css" type="text/css">
</head>
<body>
    <header>
        <h1>Angewandte Mathematik und Informatik</h1>
        <h2>Modelliere deine Wirklichkeit</h2>
    </header>

    <img src="source/nao_robots.png" alt="Roboter spielen Mädn">

    <ul id="standorte">
        <li><a href="https://www.itc.rwth-aachen.de/cms/it-center/karriere/matse-ausbildung/~letj/ueber-matse/">Aachen</a></li>
        <li><a href="https://itcc.uni-koeln.de/das-itcc/bewerbung-am-itcc/ausbildung-am-itcc/duales-studium-matse">Köln</a></li>
        <li><a href="https://www.fz-juelich.de/de/jsc/ausbildung/matse">Jülich</a></li>
    </ul>

    <a href="https://www.fh-aachen.de/studium/studiengaenge/angewandte-mathematik-und-informatik-bsc-dual">
        Matse Website
    </a>

    <form class="d-flex gap-2 mt-4">
        <input class="form-control" placeholder="Bitte E-Mail angeben" type="email">
        <button class="btn btn-primary text-nowrap">Registrieren</button>
    </form>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js"
            integrity="sha384-FKyoEForCGlyvwx9Hj09JcYn3nv7wiPVlz7YYwJrWVcXK/BmnVDxM+D2scQbITxI"
            crossorigin="anonymous"></script>
</body>
</html>
```

## styles/landingpage.css

```css
html {
    background-image: url("../source/pexels-negativespace-34676.jpg");
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
    background-attachment: fixed;
    min-height: 100%;
}

body {
    margin-left: 20%;
    margin-right: 20%;
    padding: 50px;
    background-color: rgba(255, 255, 255, 0.5);
    min-height: 100vh;
}

#standorte {
    list-style: none;
    display: flex;
    gap: 2rem;
    padding-left: 0;
}
```

## Relevante Konzepte

- HTML5-Grundstruktur mit `meta viewport` fuer responsives Design
- Bootstrap 5 Einbindung via CDN (CSS + JS Bundle)
- Bootstrap-Klassen: `d-flex`, `gap-2`, `mt-4`, `form-control`, `btn`, `btn-primary`, `text-nowrap`
- Eigenes CSS mit halbtransparentem Body-Hintergrund (`rgba`)
- Flexbox-Layout fuer Standort-Liste (`display: flex`, `gap`)
- Fullscreen-Hintergrundbild mit `background-size: cover` und `background-attachment: fixed`
