# Documentación Técnica: Proyecto Sitio Web de Negocio Local (Panadería El Trigo de Oro)

Este repositorio contiene el código fuente del proyecto entregable de HTML y CSS para el desarrollo del sitio web de un negocio local.

---

## 1. El Proyecto
* **Negocio elegido:** Panadería Artesanal "El Trigo de Oro".
* **Público objetivo:** Vecinos del barrio, familias y clientes amantes del pan artesanal de calidad que buscan una opción rápida para ver productos, precios y realizar pedidos a domicilio o para recojida.
* **Necesidad del cliente:** Actualmente el negocio realiza ventas informales por redes sociales sin un catálogo estructurado ni tarifas claras. El sitio web ofrece un espacio limpio, confiable y optimizado para celulares donde se pueden consultar los productos destacados, horarios, pasos de compra, opiniones y una tabla completa de precios junto con un formulario interactivo de pedidos.

---

## 2. Estructura
El proyecto se organiza bajo la estructura sugerida:
```text
nombre-del-negocio/
├── index.html
├── precios-pedidos.html
├── README.md
├── css/
│   └── styles.css
├── images/
│   ├── pan-artesanal.jpg
│   ├── croissant.jpg
│   ├── pastel-chocolate.jpg
│   └── hero-bg.jpg
├── video/
└── capturas/
    ├── desktop-index.png
    ├── desktop-precios.png
    ├── mobile-index-360.png
    └── mobile-precios-360.png
```
* **Etiquetas semánticas principales elegidas:**
  * `<header>`: Contiene la barra de navegación principal y el logotipo repetido en ambas páginas.
  * `<nav>`: Agrupa la lista de navegación para facilitar el acceso de lectores de pantalla.
  * `<main>`: Envuelve el contenido único y central de cada vista.
  * `<section>`: Divide el contenido en bloques temáticos coherentes (Hero, Productos, Información Útil, Opiniones, Ubicación).
  * `<article>`: Representa cada tarjeta de producto independiente, permitiendo que se entiendan por sí mismas.
  * `<aside>` / Bloques secundarios: Utilizados para destacar notas al margen e información adicional.
  * `<footer>`: Pie de página institucional con derechos reservados y enlaces externos.

---

## 3. Sistema Visual
Se definieron variables personalizadas en `:root` dentro de `styles.css` para mantener una paleta coherente y fácil de mantener:
```css
:root {
    --color-primario: #5c3a21;     /* Tono marrón panadería */
    --color-secundario: #d4a373;   /* Tono trigo suave */
    --color-acento: #bc6c25;       /* Tono terracota / oferta */
    --color-fondo: #fefae0;        /* Crema claro de fondo */
    --color-texto: #2b2b2b;        /* Gris oscuro para lectura */
    --color-blanco: #ffffff;
    --color-sombra: rgba(92, 58, 33, 0.15);
    --fuente-principal: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    --fuente-secundaria: Georgia, serif;
}
```
* **Colores:** Se seleccionaron tonos cálidos inspirados en el trigo y el horneado artesanal. Se incluye un canal alfa (`rgba(92, 58, 33, 0.15)`) para sombras suaves y un gradiente lineal en el banner hero.
* **Tipografía:** Se usa una fuente principal sans-serif de respaldo seguro para lectura fluida en párrafos y una fuente serif elegante (`Georgia`) para títulos principales.

---

## 4. Especificidad
A continuación se documentan 3 conflictos reales presentados en la hoja de estilos:

### Conflicto 1: Color del precio en oferta de la tarjeta promocional
Reglas que compiten sobre el elemento `<p>` de precio:
1. `.tarjeta__price` -> Especificidad: `(0, 1, 1)` -> `color: #2b2b2b;`
2. `.tarjeta--promo .tarjeta__price strong` -> Especificidad: `(0, 2, 1)` -> `color: var(--color-acento);`

* **Ganador:** Gana la segunda regla porque tiene mayor especificidad (`2 clases y 1 etiqueta` frente a `1 clase y 1 etiqueta`).
* *Captura de referencia en el inspector de estilos adjunta en `capturas/conflicto-1.png`.*

### Conflicto 2: Color de fondo en enlaces de navegación al hacer hover
Reglas que compiten sobre los elementos `<a>` dentro del menú:
1. `.nav__link` -> Especificidad: `(0, 1, 1)` -> `background-color: transparent;`
2. `.nav__link:hover` -> Especificidad: `(0, 2, 1)` -> `background-color: var(--color-acento);`

* **Ganador:** Gana la pseudoclase `:hover` debido a que suma una clase equivalente de pseudoclase en la especificidad `(0, 2, 1)`, superando al enlace base.
* *Captura de referencia en el inspector de estilos adjunta en `capturas/conflicto-2.png`.*

### Conflicto 3: Resolución por orden (empate de especificidad)
Reglas que compiten sobre los títulos de sección `.section__title`:
1. `.section__title` (en la sección general) -> Especificidad: `(0, 1, 0)` -> `margin-bottom: 2rem;`
2. `.seccion-tabla .section__title` (específico para la tabla) -> Especificidad: `(0, 2, 0)` -> `margin-bottom: 2rem;` (pero con diferente estilo previo).

Supongamos dos reglas con idéntica especificidad `(0, 1, 0)` definiendo `color`:
- `.section__title { color: #5c3a21; }`
- `.section__title { color: #2b2b2b; }` (definida más abajo en el archivo)

* **Ganador:** Al tener exactamente la misma especificidad `(0, 1, 0)`, gana la regla declarada **última en el archivo CSS** por orden de aparición en la cascada.
* *Captura de referencia en el inspector de estilos adjunta en `capturas/conflicto-3.png`.*

---

## 5. Herencia
* **Propiedad heredada:** `font-family` y `color` se definen en el elemento `<body>` y son heredados automáticamente por los elementos hijos como párrafos (`<p>`) y títulos dentro de las secciones, evitando repetir la declaración tipográfica.
* **Propiedad NO heredada:** `border` o `padding`. Por ejemplo, al aplicar un borde o un padding a una tarjeta (`.tarjeta`), este estilo no se transmite a las imágenes ni a los párrafos hijos que contiene; cada caja debe definir su propio espaciado interno explícitamente.

---

## 6. Modelo de Cajas
Cálculo del ancho total de la tarjeta de producto (`.tarjeta`):
* Ancho definido (`width` / `max-width`): 100% (adaptable al contenedor grid de `350px` aprox en escritorio).
* Relleno (`padding` en `.tarjeta__body`): `1.5rem` a cada lado (equivalente a `24px` izq + `24px` der = `48px`).
* Borde (`border`): `1px` a cada lado (`1px` izq + `1px` der = `2px`).
* **Ancho total de la caja:** `Ancho del contenido + 48px (padding) + 2px (bordes) = 100% + 50px`.
* Gracias a la regla universal `* { box-sizing: border-box; }`, el navegador incluye el padding y el borde dentro del ancho total declarado, evitando desbordamientos no deseados en pantallas pequeñas.
* **Márgenes que se solapan (Margin collapsing):** Se observa entre el título principal `<h1 class="hero__title">` y el subtítulo, donde los márgenes verticales superior e inferior adyacentes no se suman, sino que adoptan el valor del margen mayor.

---

## 7. Selectores
* **Pseudoclase estructural usada:** `.tabla-precios tbody tr:nth-child(even)`
* **Cómo funciona:** Cuenta todos los elementos `<tr>` hermanos dentro del elemento padre `<tbody>`. Identifica aquellos cuya posición numérica es par (`even` / 2, 4, 6...) y les aplica un color de fondo sutil con transparencia (`rgba(212, 163, 115, 0.1)`), generando un efecto cebra que facilita la lectura de la tabla de precios.

---

## 8. Responsive
* **Diseño Mobile First / @media query:** El diseño base está optimizado para celulares de hasta `360px` de ancho con una sola columna en flujo vertical flexible (`flex-direction: column`).
* **Uso del `@media`:** A partir de los `768px` de ancho (`@media (min-width: 768px)`), se activa una regla que transforma las filas flexibles en cuadrículas de tres columnas (`grid-template-columns: repeat(3, 1fr)`) para las tarjetas de productos, distribución de dos columnas en información útil y opiniones, y una barra de navegación horizontal alineada a los extremos, aprovechando el espacio de pantallas grandes de escritorio.

---

## 9. Problemas y Soluciones
1. **Problema con la barra de navegación en celulares pequeños (360px):**
   * *Por qué pasaba:* Los enlaces del menú se desbordaban horizontalmente porque estaban en una sola línea fija.
   * *Cómo se arregló:* Se configuró `.nav__list` con `flex-wrap: wrap; justify-content: center;` permitiendo que los elementos bajen de línea ordenadamente sin romper el ancho de la pantalla.
2. **Problema con la tabla de precios en pantallas móviles:**
   * *Por qué pasaba:* La tabla con cuatro columnas excedía los 360px y generaba un scroll horizontal en toda la página rompiendo el diseño.
   * *Cómo se arregló:* Se envolvió la tabla dentro de un contenedor con clase `.table-container` aplicando `overflow-x: auto;`. De esta forma, solo la tabla se desplaza horizontalmente de manera aislada sin alterar el cuerpo del sitio.

---

## 10. Capturas
Las capturas de pantalla de las vistas solicitadas se encuentran almacenadas en la carpeta `capturas/`:
* `capturas/desktop-index.png` - Página de inicio en escritorio.
* `capturas/desktop-precios.png` - Página de precios y pedidos en escritorio.
* `capturas/mobile-index-360.png` - Página de inicio en celular (360px).
* `capturas/mobile-precios-360.png` - Página de precios y pedidos en celular (360px).



*(Enlace a GitHub Pages publicado: https://github.com/diegorosasguarin/El_Trigo_de_Oro)*