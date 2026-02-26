# Ferretería Gallega — Diseño Web
**Fecha:** 2026-02-26

## Concepto
Web informativa para Ferretería Gallega. Lema: *"Si no lo encuentras aquí, es que no existe."*
Público: vecinos habituales + nuevos clientes que llegan por Google.
Personalidad: tradición, orgullo local gallego, generacional, emocional.

## Estética — "El cartel gigante"
La web entera es un cartel pintado a mano. Intencionalmente imperfecto, crudo, contundente.
Sin animaciones complejas. El impacto viene del diseño puro.

## Identidad visual

### Paleta
| Variable       | Color     | Uso                          |
|----------------|-----------|------------------------------|
| `--bg`         | `#f5f0e8` | Fondo — papel envejecido     |
| `--red`        | `#c1440e` | Primario — rojo ladrillo     |
| `--brown`      | `#5c4a1e` | Secundario — madera          |
| `--gold`       | `#e8a020` | Acento — letras doradas      |
| `--ink`        | `#1a1208` | Texto — negro cálido         |

### Tipografía
- **Bebas Neue / Anton** — títulos, todo mayúsculas, condensada, de cartel
- **Crimson Text** — cuerpo, serif clásico, periódico de provincia
- **Special Elite** — datos prácticos, efecto máquina de escribir

### Carácter visual
- Textura grain en fondo (SVG noise filter) simulando papel
- Bordes irregulares tipo brocha (`border-image` o SVG clip-path)
- Rotaciones leves: `rotate(-1deg)` / `rotate(0.5deg)` en bloques
- Sombras duras y desplazadas: `4px 4px 0 #1a1208` — sin blur, como impresión en relieve
- Sin partículas, sin glassmorphism, sin gradientes suaves

## Estructura de página (scroll vertical, una sola página)

### 1. Cabecera-cartel (100vh)
- "FERRETERÍA GALLEGA" en Bebas Neue, `clamp(5rem, 18vw, 14rem)`, full-width
- Lema en rojo debajo: *"SI NO LO ENCUENTRAS AQUÍ, ES QUE NO EXISTE"*
- Año de fundación en ocre, estilo sello, esquina inferior
- Sin navbar
- `↓` parpadeante como único elemento de navegación

### 2. "Qué tenemos" (fondo rojo ladrillo)
- Título: "LO TENEMOS TODO."
- Lista de categorías en dos columnas, tipografía grande
- Categorías: Herramientas · Fontanería · Electricidad · Pintura · Jardín · Tornillería · Madera · Adhesivos · Maquinaria
- Solo texto, sin iconos, con línea separadora

### 3. Por qué venir (fondo crema)
- Tres frases enormes con rotación leve alternada:
  - *"STOCK PARA UN PUEBLO ENTERO."*
  - *"TE CONOCEMOS POR EL NOMBRE."*
  - *"LLEVAMOS AQUÍ MÁS QUE TÚ."*

### 4. Dónde estamos
- Dos columnas: dirección + horario (Special Elite) | Google Maps (filtro sepia)
- `filter: hue-rotate(30deg) sepia(0.6)` en el mapa
- Horario con formato de cartel pegado en cristal

### 5. Pie
- Fondo `--brown`
- Teléfono en `clamp(3rem, 8vw, 6rem)`, clicable (`tel:`)
- *"Ponte Caldelas, Galicia. Desde [año]."*

## Stack técnico
- HTML + CSS + JS vanilla, archivo único `index.html`
- Google Fonts: Bebas Neue + Crimson Text + Special Elite
- Sin frameworks, sin build tools
- SVG inline para grain/textura de fondo
- Google Maps embed con filtro CSS sepia
- Mobile-first, breakpoint en 768px
