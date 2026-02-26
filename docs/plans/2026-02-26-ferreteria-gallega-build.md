# Ferretería Gallega Website — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a single-file `index.html` website for Ferretería Gallega — estética "cartel pintado a mano", solo informativa (horario, ubicación, teléfono, qué venden).

**Architecture:** Una sola página HTML con scroll vertical. Todo el CSS y JS inline en el mismo archivo. Sin frameworks ni build tools. Impacto visual puro mediante tipografía, color y textura — sin animaciones complejas.

**Tech Stack:** HTML5, CSS3 custom properties, JS vanilla, Google Fonts (Bebas Neue + Crimson Text + Special Elite), Google Maps embed.

**Design doc:** `docs/plans/2026-02-26-ferreteria-gallega-design.md`

---

## PALETA Y TOKENS

```css
--bg:    #f5f0e8;   /* papel envejecido */
--red:   #c1440e;   /* rojo ladrillo */
--brown: #5c4a1e;   /* madera */
--gold:  #e8a020;   /* letras doradas */
--ink:   #1a1208;   /* negro cálido */
```

## REGLAS DE ESTILO GLOBALES

- Sombras: `4px 4px 0 var(--ink)` — duras, sin blur, como impresión en relieve
- Rotaciones: `rotate(-1deg)` / `rotate(0.8deg)` — alternadas por bloque
- Grain: SVG filter `feTurbulence` + `feColorMatrix` como pseudo-elemento `::before` en `body`
- Bordes: `3px solid var(--ink)` — sin border-radius salvo sellos (border-radius: 2px)
- Tipografía títulos: Bebas Neue, `text-transform: uppercase`, `letter-spacing: 0.02em`
- Tipografía cuerpo: Crimson Text, serif
- Tipografía datos: Special Elite, monospace-feel

---

### Task 1: Estructura base y reset

**Files:**
- Create: `ferreteria/index.html`

**Step 1: Crear el archivo con estructura HTML5, meta tags, Google Fonts y CSS reset**

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ferretería Gallega — Si no lo encuentras aquí, es que no existe</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Crimson+Text:ital,wght@0,400;0,600;1,400&family=Special+Elite&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --bg: #f5f0e8; --red: #c1440e; --brown: #5c4a1e;
      --gold: #e8a020; --ink: #1a1208;
    }
    html { scroll-behavior: smooth; }
    body {
      background: var(--bg);
      color: var(--ink);
      font-family: 'Crimson Text', Georgia, serif;
      overflow-x: hidden;
      position: relative;
    }
    /* Grain texture */
    body::before {
      content: '';
      position: fixed; inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none; z-index: 9999;
      opacity: 0.4;
    }
  </style>
</head>
<body>
  <!-- secciones aquí -->
</body>
</html>
```

**Step 2: Commit**
```bash
git add ferreteria/index.html
git commit -m "feat: base HTML structure and global styles"
```

---

### Task 2: Cabecera-cartel (hero, 100vh)

**Files:**
- Modify: `ferreteria/index.html` — añadir sección hero + CSS

**Step 1: HTML del hero**

```html
<section class="hero">
  <div class="hero-inner">
    <div class="hero-stamp">EST. 1978</div>
    <h1 class="hero-title">Ferretería<br>Gallega</h1>
    <p class="hero-lema">"Si no lo encuentras aquí,<br>es que no existe."</p>
    <a class="hero-scroll" href="#tenemos">↓</a>
  </div>
</section>
```

**Step 2: CSS del hero**

```css
.hero {
  min-height: 100vh;
  background: var(--bg);
  display: flex; align-items: center; justify-content: center;
  text-align: center;
  padding: 60px 24px;
  position: relative;
  border-bottom: 4px solid var(--ink);
}
.hero-inner {
  display: flex; flex-direction: column;
  align-items: center; gap: 24px;
  transform: rotate(-0.5deg);
}
.hero-stamp {
  font-family: 'Special Elite', monospace;
  font-size: 13px; letter-spacing: .3em;
  color: var(--gold);
  border: 2px solid var(--gold);
  padding: 6px 16px;
  text-transform: uppercase;
  box-shadow: 2px 2px 0 var(--brown);
}
.hero-title {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(5rem, 18vw, 13rem);
  line-height: 0.9;
  letter-spacing: 0.02em;
  color: var(--ink);
  text-shadow: 5px 5px 0 var(--red);
}
.hero-lema {
  font-family: 'Crimson Text', serif;
  font-size: clamp(1.1rem, 3vw, 1.6rem);
  font-style: italic;
  color: var(--brown);
  max-width: 500px;
  line-height: 1.4;
}
.hero-scroll {
  margin-top: 32px;
  font-family: 'Bebas Neue', sans-serif;
  font-size: 2rem; color: var(--ink);
  text-decoration: none;
  animation: blink 1.4s step-end infinite;
}
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
```

**Step 3: Commit**
```bash
git commit -m "feat: hero cartel section"
```

---

### Task 3: Sección "Lo tenemos todo"

**Files:**
- Modify: `ferreteria/index.html`

**Step 1: HTML**

```html
<section class="tenemos" id="tenemos">
  <div class="tenemos-inner">
    <h2 class="tenemos-titulo">Lo tenemos<br>todo.</h2>
    <div class="tenemos-grid">
      <span>Herramientas</span><span>Fontanería</span>
      <span>Electricidad</span><span>Pintura</span>
      <span>Jardín</span><span>Tornillería</span>
      <span>Madera</span><span>Adhesivos</span>
      <span>Maquinaria</span><span>Ferretería general</span>
    </div>
  </div>
</section>
```

**Step 2: CSS**

```css
.tenemos {
  background: var(--red);
  color: var(--bg);
  padding: 100px 48px;
  border-bottom: 4px solid var(--ink);
}
.tenemos-inner {
  max-width: 900px; margin: 0 auto;
  transform: rotate(0.4deg);
}
.tenemos-titulo {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(4rem, 12vw, 9rem);
  line-height: 0.9;
  color: var(--bg);
  text-shadow: 4px 4px 0 var(--ink);
  margin-bottom: 60px;
}
.tenemos-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0;
}
.tenemos-grid span {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(1.6rem, 4vw, 2.8rem);
  letter-spacing: 0.04em;
  padding: 18px 0;
  border-top: 2px solid rgba(245,240,232,.3);
  color: var(--bg);
}
.tenemos-grid span:nth-child(odd) { padding-right: 40px; border-right: 2px solid rgba(245,240,232,.3); }
.tenemos-grid span:nth-child(even) { padding-left: 40px; }
```

**Step 3: Commit**
```bash
git commit -m "feat: categories section"
```

---

### Task 4: Sección "Por qué venir"

**Files:**
- Modify: `ferreteria/index.html`

**Step 1: HTML**

```html
<section class="porqué">
  <div class="porqué-inner">
    <p class="frase frase--left">Stock para<br>un pueblo entero.</p>
    <p class="frase frase--right">Te conocemos<br>por el nombre.</p>
    <p class="frase frase--left">Llevamos aquí<br>más que tú.</p>
  </div>
</section>
```

**Step 2: CSS**

```css
.porqué {
  background: var(--bg);
  padding: 100px 48px;
  border-bottom: 4px solid var(--ink);
}
.porqué-inner {
  max-width: 900px; margin: 0 auto;
  display: flex; flex-direction: column; gap: 0;
}
.frase {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3rem, 9vw, 7rem);
  line-height: 0.95;
  color: var(--ink);
  padding: 32px 0;
  border-bottom: 3px solid var(--ink);
}
.frase:first-child { border-top: 3px solid var(--ink); }
.frase--left  { transform: rotate(-0.8deg); text-align: left; }
.frase--right { transform: rotate(0.6deg);  text-align: right; color: var(--red); }
```

**Step 3: Commit**
```bash
git commit -m "feat: why us section"
```

---

### Task 5: Sección "Dónde estamos"

**Files:**
- Modify: `ferreteria/index.html`

**Sustituir coordenadas del Maps con las reales de la ferretería.**

**Step 1: HTML**

```html
<section class="donde" id="donde">
  <div class="donde-inner">
    <div class="donde-info">
      <h2 class="donde-titulo">Encuéntranos.</h2>
      <div class="donde-datos">
        <p class="dato-label">Dirección</p>
        <p class="dato-valor">Rúa Principal, 12<br>Ponte Caldelas, Pontevedra</p>
        <p class="dato-label">Teléfono</p>
        <p class="dato-valor"><a href="tel:+34986000000">986 000 000</a></p>
        <p class="dato-label">Horario</p>
        <p class="dato-valor">
          Lun–Vie &nbsp;9:00 – 14:00 / 16:00 – 20:00<br>
          Sábado &nbsp;&nbsp;9:00 – 14:00<br>
          Domingo Cerrado
        </p>
      </div>
    </div>
    <div class="donde-mapa">
      <iframe
        src="https://maps.google.com/maps?q=Ponte+Caldelas+Pontevedra&t=&z=15&ie=UTF8&iwloc=&output=embed"
        loading="lazy" title="Localización"></iframe>
    </div>
  </div>
</section>
```

**Step 2: CSS**

```css
.donde {
  background: var(--brown);
  color: var(--bg);
  padding: 100px 48px;
  border-bottom: 4px solid var(--ink);
}
.donde-inner {
  max-width: 1100px; margin: 0 auto;
  display: grid; grid-template-columns: 1fr 1fr; gap: 60px;
  align-items: start;
}
.donde-titulo {
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3rem, 7vw, 6rem);
  color: var(--gold);
  text-shadow: 3px 3px 0 var(--ink);
  margin-bottom: 40px;
}
.dato-label {
  font-family: 'Special Elite', monospace;
  font-size: 11px; letter-spacing: .2em;
  text-transform: uppercase;
  color: var(--gold);
  margin-top: 24px; margin-bottom: 6px;
}
.dato-valor {
  font-family: 'Special Elite', monospace;
  font-size: 1.05rem; line-height: 1.7;
  color: var(--bg);
}
.dato-valor a { color: var(--gold); text-decoration: none; }
.donde-mapa iframe {
  width: 100%; height: 380px; border: 3px solid var(--ink);
  box-shadow: 6px 6px 0 var(--ink);
  filter: sepia(0.6) hue-rotate(10deg) contrast(1.05);
}
@media (max-width: 768px) {
  .donde-inner { grid-template-columns: 1fr; }
}
```

**Step 3: Commit**
```bash
git commit -m "feat: location and hours section"
```

---

### Task 6: Footer con teléfono gigante

**Files:**
- Modify: `ferreteria/index.html`

**Step 1: HTML**

```html
<footer class="pie">
  <p class="pie-label">Llámanos</p>
  <a class="pie-telefono" href="tel:+34986000000">986 000 000</a>
  <p class="pie-lugar">Ponte Caldelas, Galicia &mdash; Desde 1978.</p>
</footer>
```

**Step 2: CSS**

```css
.pie {
  background: var(--ink);
  color: var(--bg);
  padding: 80px 48px 60px;
  text-align: center;
}
.pie-label {
  font-family: 'Special Elite', monospace;
  font-size: 12px; letter-spacing: .3em;
  text-transform: uppercase;
  color: var(--gold);
  margin-bottom: 16px;
}
.pie-telefono {
  display: block;
  font-family: 'Bebas Neue', sans-serif;
  font-size: clamp(3rem, 10vw, 7rem);
  color: var(--bg);
  text-decoration: none;
  line-height: 1;
  text-shadow: 4px 4px 0 var(--red);
  transition: color .2s;
}
.pie-telefono:hover { color: var(--gold); }
.pie-lugar {
  margin-top: 32px;
  font-family: 'Crimson Text', serif;
  font-style: italic;
  font-size: 1rem;
  color: rgba(245,240,232,.45);
}
```

**Step 3: Commit**
```bash
git commit -m "feat: footer with giant phone number"
```

---

### Task 7: Ajustes mobile y polish final

**Files:**
- Modify: `ferreteria/index.html`

**Step 1: Media queries globales**

```css
@media (max-width: 768px) {
  .hero-title { text-shadow: 3px 3px 0 var(--red); }
  .tenemos { padding: 60px 24px; }
  .tenemos-grid { grid-template-columns: 1fr; }
  .tenemos-grid span:nth-child(odd) { border-right: none; padding-right: 0; }
  .tenemos-grid span:nth-child(even) { padding-left: 0; }
  .porqué { padding: 60px 24px; }
  .donde { padding: 60px 24px; }
  .pie { padding: 60px 24px 40px; }
}
```

**Step 2: Añadir `<link rel="icon">` con favicon apropiado**

```html
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'><rect width='32' height='32' rx='4' fill='%23c1440e'/><text y='24' x='4' font-size='22' font-family='sans-serif' fill='%23f5f0e8'>🔧</text></svg>">
```

**Step 3: Ajustar año de fundación, teléfono real y dirección real**

Buscar y reemplazar los placeholders:
- `1978` → año real de fundación
- `986 000 000` → teléfono real
- `Rúa Principal, 12` → dirección real

**Step 4: Deploy**
```bash
netlify deploy --dir ferreteria --prod
```

**Step 5: Commit final**
```bash
git add ferreteria/
git commit -m "feat: complete Ferretería Gallega website"
```

---

## Checklist de revisión visual

- [ ] El título ocupa toda la anchura en desktop sin overflow
- [ ] Rotaciones visibles pero sutiles (no marean en mobile)
- [ ] Grain texture se ve en pantalla sin pixelado excesivo
- [ ] Mapa con filtro sepia carga correctamente
- [ ] Teléfono en footer es clicable en móvil (`tel:`)
- [ ] Contraste rojo/crema supera WCAG AA en secciones de texto
- [ ] Sin scroll horizontal en ningún breakpoint
