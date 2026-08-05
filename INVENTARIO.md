# INVENTARIO — puntos de personalización

Relevamiento en **modo solo lectura** del proyecto (ningún archivo del sitio fue
modificado). El objetivo es tener el mapa completo de qué hay que cambiar para
el cliente nuevo, incluyendo lo que **no** está marcado.

Datos del negocio anterior que se usan como referencia en todo el documento:

| Dato | Valor actual |
| --- | --- |
| Nombre | `IPHONE ALLEN` |
| WhatsApp | `5492613900039` |
| Dirección | `Río Cuarto 2341, Allen, Río Negro` |
| Localidad | `Allen, Río Negro` |
| Instagram | `iphone.allen` |
| Dominio | `https://iphone-allen.vercel.app/` |
| Correo | `[CORREO]` (placeholder, no es dato real del anterior) |
| Inicial del favicon | `A` |

---

## Marcadores `>>> PERSONALIZAR <<<`

**Total: 21 marcadores en el código** (`index.html` 4 · `iphones.html` 1 ·
`mac.html` 1 · `ipad.html` 1 · `accesorios.html` 1 · `productos.html` 1 ·
`app.js` 11 · `favicon.svg` 1).

Además hay 3 menciones de la cadena dentro de `PERSONALIZAR.md` (líneas 11, 204
y 231), que son texto explicativo del checklist y **no** cuentan como puntos a
editar. Búsqueda literal en todo el repo: 24 líneas; puntos reales: **21**.

| # | Archivo | Línea | Qué dato pide | Valor actual del cliente anterior |
| --- | --- | --- | --- | --- |
| 1 | `index.html` | 52 | Bloque de cabecera: title, meta description y Open Graph de la página | `IPHONE ALLEN`, `Allen, Río Negro`, `https://iphone-allen.vercel.app/` |
| 2 | `index.html` | 121 | Localidad y zona de envíos del eyebrow del hero | `Allen, Río Negro · Envíos a ciudades vecinas` (línea 124) |
| 3 | `index.html` | 148 | Dirección del local en el hero | `Río Cuarto 2341, Allen, Río Negro` (línea 152) |
| 4 | `index.html` | 194 | Cuotas de la franja de promo + tarjetas (provisorio) | `3, 6 y 12 cuotas sin interés` (línea 205); tarjetas desde `TARJETAS` |
| 5 | `iphones.html` | 52 | Ídem #1 para esta página | `iPhones — IPHONE ALLEN \| Allen, Río Negro` |
| 6 | `mac.html` | 52 | Ídem #1 para esta página | `Mac — IPHONE ALLEN \| Allen, Río Negro` |
| 7 | `ipad.html` | 52 | Ídem #1 para esta página | `iPad — IPHONE ALLEN \| Allen, Río Negro` |
| 8 | `accesorios.html` | 52 | Ídem #1 para esta página | `Accesorios — IPHONE ALLEN \| Allen, Río Negro` |
| 9 | `productos.html` | 52 | Ídem #1 para esta página | `Todos los productos — IPHONE ALLEN \| Allen, Río Negro` |
| 10 | `app.js` | 67 | Aviso general del bloque de configuración (no es un dato) | — |
| 11 | `app.js` | 75 | Nombre del negocio (`NEGOCIO`) | `IPHONE ALLEN` (línea 82) |
| 12 | `app.js` | 84 | WhatsApp (`WHATSAPP`) | `5492613900039` (línea 87) |
| 13 | `app.js` | 93 | Dirección del local (`DIRECCION`) | `Río Cuarto 2341, Allen, Río Negro` (línea 99) |
| 14 | `app.js` | 101 | URL pública del sitio (`SITIO`) | `https://iphone-allen.vercel.app/` (línea 107) |
| 15 | `app.js` | 128 | Instagram y correo (`CONTACTO`) | `instagram: 'iphone.allen'` (142), `email: '[CORREO]'` (143) |
| 16 | `app.js` | 147 | Entrega estimada (`ENTREGA_ESTIMADA` / `ENTREGA_NOTA`) | `'12 a 36 hs'` (156) y `'Coordinamos por WhatsApp'` (157) |
| 17 | `app.js` | 160 | Tarjetas de la franja de promo (`TARJETAS`) | `['Visa', 'Mastercard', 'Cabal']` (169) |
| 18 | `app.js` | 186 | Formas de pago (`FORMAS_PAGO`) | Efectivo / Transferencia / Tarjeta con detalle provisorio (193-197) |
| 19 | `app.js` | 200 | Preguntas frecuentes (`FAQ`) | 7 preguntas; la de envíos nombra `Allen` (213) |
| 20 | `app.js` | 999 | Variante del separador del logotipo (`LOGO_VARIANTE`) | `'regla'` (1007) |
| 21 | `favicon.svg` | 2 | Inicial del negocio dentro del `<text>` | `A` (línea 17) |

---

## Ocurrencias fuera de marcador

**Esta es la sección importante: son las trampas.** Datos del cliente anterior
que están en el código y que **no** tienen un `>>> PERSONALIZAR <<<` al lado.
Quien siga solamente los marcadores se los pierde.

### Trampas dentro de los seis HTML

| Archivo | Línea | Qué hay | Por qué es trampa |
| --- | --- | --- | --- |
| `index.html` | 73 | Comentario: `og:url y og:image apuntan a https://iphone-allen.vercel.app/` | El marcador está en la línea 52, arriba; el dominio viejo queda escrito también acá |
| `index.html` | 91 | Comentario `CÓMO PERSONALIZAR ESTA PÁGINA`: cita `"IPHONE ALLEN"` | Bloque aparte, sin marcador. **Es la 4ta ocurrencia del nombre en index.html** |
| `iphones.html` · `mac.html` · `ipad.html` · `accesorios.html` · `productos.html` | 73 (cada uno) | Mismo comentario con el dominio viejo | Ídem: 5 archivos más con `iphone-allen.vercel.app` en comentario |
| Los 6 HTML | 80 | `og:description` contiene `Allen, Río Negro` | El marcador de la línea 52 lo cubre en teoría, pero la localidad acá se escapa fácil porque en esta etiqueta **no** aparece el nombre del negocio |
| Los 6 HTML | 81 y 82 | `og:image` y `og:url` con `https://iphone-allen.vercel.app/...` | Cada página tiene su propia URL: son 12 strings distintos a cambiar |

### Trampas dentro de `app.js`

| Línea | Qué hay | Por qué es trampa |
| --- | --- | --- |
| 2 | Encabezado del archivo: `IPHONE ALLEN — lógica de la tienda` | Primer comentario del archivo, sin marcador |
| 86 | Comentario del WhatsApp: `261 = Mendoza` | Explica la característica del número viejo; además contradice la dirección (Río Negro) |
| 213 | `FAQ`: `'Hacemos envíos a Allen y ciudades vecinas...'` | El marcador está en la línea 200, 13 líneas más arriba: el dato concreto queda lejos del cartel |
| 1030 | Comentario `DIRECCIÓN PROVISORIA - confirmar con el cliente antes de publicar` en `htmlFooter()` | Sin marcador; el dato sale de `DIRECCION` pero el recordatorio está acá |
| 3432 | Comentario del contador de pestaña: `"(2) iPhones — IPHONE ALLEN…"` | Comentario a 3400 líneas del bloque de configuración |

### Trampas fuera de HTML y `app.js`

| Archivo | Línea | Qué hay | Por qué es trampa |
| --- | --- | --- | --- |
| `styles.css` | 2 | `IPHONE ALLEN — hoja de estilos única` | `PERSONALIZAR.md` dice explícitamente **no tocar `styles.css`**, así que nadie lo abre; el nombre viejo queda ahí |
| `favicon.svg` | 3 | El comentario nombra `"Allen"` | Tiene marcador en la línea 2, pero el texto del comentario también lleva el nombre viejo y hay que limpiarlo |
| `favicon-32.png` | binario | Render de la letra `A` | No se puede grepear: hay que regenerarlo a mano |
| `apple-touch-icon.png` | binario | Render de la letra `A` | Ídem |
| `README.md` | 1, 3, 47, 70, 111 | Nombre, localidad, dirección completa y dominio viejo | `PERSONALIZAR.md` dice **no tocar el README**, pero es documentación que queda con datos ajenos |
| `PERSONALIZAR.md` | 66, 202 | El propio checklist cita `Allen, Río Negro` y `IPHONE ALLEN` | Es ejemplo intencional, pero conviene limpiarlo al cerrar el trabajo |

### Lo que **no** apareció (buscado y descartado)

- No hay ningún número de teléfono del cliente anterior fuera de
  `app.js:87`. Los otros dígitos largos del proyecto son `placeholder` de
  formulario (`app.js:1144`, `Ej: 2985551234`), que es texto de ejemplo del
  campo, no un contacto real.
- No hay ninguna dirección de correo real: `CONTACTO.email` está en
  `'[CORREO]'` y el único `@` del código es el `placeholder` `tu@email.com`
  (`app.js:1150`).
- No hay más redes sociales que Instagram (`app.js:142`): no hay Facebook,
  TikTok, X ni YouTube en ninguna parte.
- `productos.json` **no** contiene ninguna mención del negocio anterior
  (0 ocurrencias de `allen`): es catálogo genérico con precios inventados.
- No hay atributos `alt` escritos a mano en los HTML: los `alt` los genera
  `app.js` a partir de `p.nombre` (`app.js:541` y `app.js:3243`), así que no
  arrastran el nombre del comercio.

---

## Nombre del comercio por página

Se cuentan dos cosas distintas por página:

- **Marca** = la cadena `IPHONE ALLEN`.
- **Total `allen`** = todas las ocurrencias sin distinguir mayúsculas, o sea la
  marca + la localidad `Allen` + el slug del dominio `iphone-allen`.

### `index.html` — marca **4** · total `allen` **13**

| Línea | Etiqueta / lugar | Contenido |
| --- | --- | --- |
| 64 | `<title>` | `IPHONE ALLEN — iPhones y productos Apple en Allen, Río Negro` |
| 65 | `<meta name="description">` | `IPHONE ALLEN: revendedora ... en Allen, Río Negro` |
| 73 | comentario HTML | `https://iphone-allen.vercel.app/` |
| 79 | `<meta property="og:title">` | `IPHONE ALLEN — iPhones y productos Apple en Allen, Río Negro` |
| 80 | `<meta property="og:description">` | `... Local en Allen, Río Negro ...` (localidad, **sin** la marca) |
| 81 | `<meta property="og:image">` | `https://iphone-allen.vercel.app/img/iphone-16-pro-max-256.jpg` |
| 82 | `<meta property="og:url">` | `https://iphone-allen.vercel.app/` |
| 91 | comentario HTML | `"IPHONE ALLEN" aparece en el <title>...` |
| 124 | `<p class="eyebrow">` | `Allen, Río Negro · Envíos a ciudades vecinas` |
| 152 | `<p class="hero__direccion">` | `Río Cuarto 2341, Allen, Río Negro` |

### `iphones.html` — marca **3** · total `allen` **10**

| Línea | Etiqueta | Contenido |
| --- | --- | --- |
| 64 | `<title>` | `iPhones — IPHONE ALLEN \| Allen, Río Negro` |
| 65 | `<meta name="description">` | `iPhones liberados ... en IPHONE ALLEN, Allen, Río Negro` |
| 73 | comentario HTML | `https://iphone-allen.vercel.app/` |
| 79 | `og:title` | `iPhones — IPHONE ALLEN \| Allen, Río Negro` |
| 80 | `og:description` | `... local en Allen, Río Negro ...` (sin la marca) |
| 81 | `og:image` | `https://iphone-allen.vercel.app/img/iphone-16-pro-max-256.jpg` |
| 82 | `og:url` | `https://iphone-allen.vercel.app/iphones.html` |

### `mac.html` — marca **3** · total `allen` **10**

Mismas líneas y misma estructura que `iphones.html`:
64 `<title>` · 65 `description` · 73 comentario · 79 `og:title` ·
80 `og:description` (solo localidad) · 81 `og:image`
(`.../img/macbook-air-m3-13.jpg`) · 82 `og:url` (`.../mac.html`).

### `ipad.html` — marca **3** · total `allen` **10**

64 `<title>` · 65 `description` · 73 comentario · 79 `og:title` ·
80 `og:description` (solo localidad) · 81 `og:image`
(`.../img/ipad-air-m2-11.jpg`) · 82 `og:url` (`.../ipad.html`).

### `accesorios.html` — marca **3** · total `allen` **10**

64 `<title>` · 65 `description` · 73 comentario · 79 `og:title` ·
80 `og:description` (solo localidad) · 81 `og:image`
(`.../img/airpods-pro-2.jpg`) · 82 `og:url` (`.../accesorios.html`).

### `productos.html` — marca **3** · total `allen` **10**

64 `<title>` · 65 `description` · 73 comentario · 79 `og:title` ·
80 `og:description` (solo localidad) · 81 `og:image`
(`.../img/iphone-16-pro-max-256.jpg`) · 82 `og:url` (`.../productos.html`).

### Notas sobre etiquetas ausentes

- **No existe `og:site_name` en ninguna de las seis páginas.** Tampoco
  `application-name` ni `theme-color`. Si se quiere agregar `og:site_name`, hay
  que crearlo, no editarlo.
- **El header y el footer no están en el HTML.** El logotipo lo genera `app.js`
  (`htmlLogo()`, línea 1009) a partir de la constante `NEGOCIO`, y se inyecta en
  los `<div id="app-header">` / `<div id="app-footer">` de cada página. Por eso
  el nombre aparece en el sitio muchas más veces de las que se ven en el HTML,
  pero se cambia en un solo lugar.
- **Totales del proyecto:** marca `IPHONE ALLEN` = 4+3+3+3+3+3 = **19** en los
  HTML, más 3 en `app.js` (líneas 2, 82, 3432), 1 en `styles.css`, 2 en
  `README.md` y 1 en `PERSONALIZAR.md`.

---

## Estructura de `productos.json`

Archivo con **dos arrays de primer nivel**: `productos` (15 entradas) y
`combos` (3 entradas).

### Esquema de un producto

| Campo | Tipo | Obligatorio | Qué hace |
| --- | --- | --- | --- |
| `id` | string | sí | Único, sin espacios ni acentos. **También define la foto por convención** |
| `nombre` | string | sí | Se muestra tal cual; alimenta el `alt` de la imagen |
| `categoria` | string | sí | `iPhone` · `Mac` · `iPad` · `Accesorios`. Orden fijo en `ORDEN_CATEGORIAS` (`app.js:114`) |
| `subcategoria` | string | no | **Sólo en Accesorios**: `Auriculares`, `Relojes`, `Cargadores`. Alimenta el desplegable del menú |
| `precio` | número | sí | Sin puntos ni `$`; se formatea solo en pesos |
| `precioAnterior` | número | no | Se muestra tachado |
| `destacado` | bool | no | `true` => entra al carrusel de la home |
| `principal` | bool | no | `true` => es la tarjeta grande de su categoría. Si hay varios, se usa el primero y avisa por consola (`app.js:2088`) |
| `etiqueta` | string | no | Sólo `"nuevo"` (pinta *Nuevo ingreso*) u `"oferta"` (*Oferta*). Otro valor no muestra nada (`app.js:613`) |
| `stock` | número | no | `0` no oculta: muestra *Sin stock* y cambia el botón. Sin el campo se asume que hay |
| `specs` | array de strings | sí | Lista corta de la tarjeta |
| `detalle` | objeto | sí | Pares clave-valor de la ficha del modal, en el orden en que se escriben |
| `imagen` | string | no | Ruta explícita. **Le gana a la convención**; `""` equivale a no tenerlo |
| `condicion` | string | no | **`"usado"` es lo que marca un producto usado** |
| `anio` | número | no | Sólo usados: ordena la sección del más nuevo al más viejo. Sin `anio` van al final |
| `estado` | objeto | no | Sólo usados: informe del equipo. Campos opcionales `bateria` (número, se muestra `%`), `pantalla`, `carcasa`, `uso`, `reparaciones`, `accesorios`. El que falta no se muestra |
| `equivaleNuevo` | string | no | Sólo usados: `id` del mismo modelo nuevo, para calcular el ahorro |

### Qué campo marca "usado"

`"condicion": "usado"`. Sin ese campo se asume nuevo (`esUsado()`,
`app.js:685`). Un usado es **una entrada propia y duplicada** del catálogo, no un
atributo del producto nuevo: el mismo modelo puede estar sellado y usado a la
vez, con precios distintos. Si ningún producto tiene `condicion: "usado"`, la
sección de usados desaparece sola.

Hoy hay **3 usados**: `iphone-14-128-usado`, `iphone-13-128-usado` y
`macbook-air-m3-13-usado`.

### Qué campo marca "combo"

Un combo **no vive en `productos`**: es una entrada del array `combos`, con
esquema propio:

| Campo | Tipo | Qué hace |
| --- | --- | --- |
| `id` | string | Único |
| `categoria` | string | En qué sección se muestra como tarjeta principal |
| `productos` | array de 2 ids | Tienen que existir en el catálogo |
| `descuento` | número | Porcentaje sobre la suma de los dos precios |
| `etiqueta` | string | Por defecto `"combo"` |

**El precio no se escribe: se calcula** (`construirCombos()`, `app.js:441`) como
`suma * (1 - descuento/100)`, redondeado; `precioAnterior` queda como la suma sin
descuento para mostrarla tachada. El stock del combo es el mínimo de los dos
productos. En el objeto resultante la marca interna es la propiedad `esCombo:
true` más la lista `items`.

Reglas que hacen que un combo se ignore (avisa por consola y sigue):
no son exactamente dos ids, algún id no existe en el catálogo, o **alguno de los
dos productos es usado**.

Combos actuales: `combo-iphone-16-magsafe` (10 %), `combo-macbook-air-airpods`
(8 %) y `combo-ipad-air-airpods` (8 %).

### Cómo se referencian las imágenes de `img/`

`rutaImagen()` (`app.js:530`):

```js
return p.imagen || ('img/' + p.id + '.jpg');
```

1. Si el producto trae `"imagen"` no vacío, esa ruta gana.
2. Si no, se arma por convención: `img/<id>.jpg`.

O sea que para dar de alta la foto de un producto alcanza con guardarla en
`img/` con el nombre del `id` — no hay que tocar el JSON. El campo `"imagen"`
existe justamente para el caso de dos productos que comparten foto (los tres
usados apuntan a la foto del modelo nuevo). Si el archivo no existe o no carga,
un listener de `error` lo reemplaza por el placeholder gris con el nombre
adentro; no es un error, es el diseño previsto.

### Cantidades

- **15 productos** (12 nuevos + 3 usados).
- **3 combos**.
- Por categoría: iPhone 7 · Mac 3 · iPad 2 · Accesorios 3.

---

## Inventario de `img/`

**12 archivos**, todos `.jpg`, todos fotos de producto. No hay en esa carpeta
ningún logo, favicon ni imagen de Open Graph propia.

| Archivo | KB | Uso |
| --- | --- | --- |
| `iphone-16-pro-max-256.jpg` | 37 | Producto `iphone-16-pro-max-256`. **Además es el `og:image` de `index.html`, `iphones.html` y `productos.html`** |
| `iphone-15-pro-256.jpg` | 104 | Producto `iphone-15-pro-256` |
| `iphone-15-128.jpg` | 44 | Producto `iphone-15-128` |
| `iphone-14-128.jpg` | 38 | Producto `iphone-14-128` **y compartida** por `iphone-14-128-usado` (campo `imagen`) |
| `iphone-13-128.jpg` | 34 | Producto `iphone-13-128` **y compartida** por `iphone-13-128-usado` |
| `macbook-air-m3-13.jpg` | 64 | Producto `macbook-air-m3-13`, **compartida** por `macbook-air-m3-13-usado` y **`og:image` de `mac.html`** |
| `macbook-pro-14-m4.jpg` | 38 | Producto `macbook-pro-14-m4` |
| `ipad-air-m2-11.jpg` | 77 | Producto `ipad-air-m2-11` y **`og:image` de `ipad.html`** |
| `ipad-10-64.jpg` | 42 | Producto `ipad-10-64` |
| `airpods-pro-2.jpg` | 23 | Producto `airpods-pro-2` y **`og:image` de `accesorios.html`** |
| `apple-watch-s10-42.jpg` | 41 | Producto `apple-watch-s10-42` |
| `magsafe-15w.jpg` | 21 | Producto `magsafe-15w` |

Cobertura: los 12 productos nuevos tienen foto por convención; los 3 usados
reutilizan la del modelo nuevo. **Ningún producto queda sin imagen.**

Imágenes de marca, fuera de `img/` (raíz del proyecto):

| Archivo | KB | Uso |
| --- | --- | --- |
| `favicon.svg` | 1.1 | Favicon principal. Letra `A` jade `#5DCAA5` sobre `#111418` |
| `favicon-32.png` | 1.2 | Favicon 32×32 para navegadores sin SVG |
| `apple-touch-icon.png` | 3.8 | Ícono 180×180 de "agregar a pantalla de inicio" |

Los tres suman ~6 KB, que es la referencia de peso que da `PERSONALIZAR.md`.

**Falta:** no existe todavía una imagen propia de 1200 × 630 px para Open Graph;
las seis páginas usan fotos cuadradas de producto como `og:image`.

---

## Meta robots `noindex`

Confirmado en las **seis** páginas, siempre en la misma línea y con el mismo
contenido `noindex, nofollow`:

| Archivo | Línea | Contenido |
| --- | --- | --- |
| `index.html` | 70 | `<meta name="robots" content="noindex, nofollow">` |
| `iphones.html` | 70 | `<meta name="robots" content="noindex, nofollow">` |
| `mac.html` | 70 | `<meta name="robots" content="noindex, nofollow">` |
| `ipad.html` | 70 | `<meta name="robots" content="noindex, nofollow">` |
| `accesorios.html` | 70 | `<meta name="robots" content="noindex, nofollow">` |
| `productos.html` | 70 | `<meta name="robots" content="noindex, nofollow">` |

En los seis archivos la etiqueta viene precedida por el comentario de las líneas
67-69 que explica por qué está. Las menciones de `noindex` en `PERSONALIZAR.md`
(línea 193) y `README.md` (línea 65) son documentación, no etiquetas activas.

**Ojo:** estas seis líneas **no tienen** marcador `>>> PERSONALIZAR <<<`. Si se
publica sin sacarlas, el sitio funciona perfecto pero no aparece en Google.

---

## Discrepancias

### Lo que `PERSONALIZAR.md` dice y no se verifica en el código

1. **"En cada una hay que cambiarlo en cuatro lugares: `<title>`,
   `<meta name="description">`, `og:title`, `og:description`"** (§1, líneas
   27-31). **El nombre del negocio no está en `og:description` de ninguna de las
   seis páginas** — esa etiqueta lleva la localidad (`Allen, Río Negro`) pero no
   la marca. Los lugares con el nombre son tres por página (title, description,
   og:title), y cuatro en `index.html` por el comentario extra. El checklist
   igual sirve porque `og:description` sí hay que editarla, pero por la
   localidad, no por el nombre.

2. **"`app.js`, constante `NEGOCIO` (cerca de la línea 68)"** (§1, línea 18).
   `NEGOCIO` está en la **línea 82**; la 68 cae dentro del comentario de
   apertura del bloque de configuración. Desfase de 14 líneas.

3. **`README.md:47` afirma** que la dirección "está marcada con el comentario
   `DIRECCIÓN PROVISORIA` en `index.html` y en `app.js`". En `index.html` **no
   existe** ese comentario: la dirección del hero (línea 152) está marcada con un
   `>>> PERSONALIZAR <<<` (línea 148). El comentario `DIRECCIÓN PROVISORIA`
   existe sólo en `app.js:1030`.

### Datos del cliente anterior que `PERSONALIZAR.md` no menciona

4. **`styles.css` línea 2 tiene el nombre viejo** (`IPHONE ALLEN — hoja de
   estilos única`). El checklist pone `styles.css` explícitamente en "Lo que NO
   hay que tocar" (§ *Lo que NO hay que tocar*), así que nadie lo va a abrir.
   Es sólo un comentario, no afecta al render, pero queda el nombre ajeno en el
   archivo.

5. **`app.js` línea 2 tiene el nombre viejo** en el encabezado del archivo, sin
   marcador y fuera del bloque de configuración.

6. **`app.js` línea 3432** repite el nombre viejo en el comentario del contador
   de la pestaña, a 3400 líneas de distancia de la configuración.

7. **`README.md` conserva todos los datos del negocio anterior** (líneas 1, 3,
   47, 70, 111: nombre, localidad, dirección completa y dominio). El checklist
   lo lista como "no tocar", pero es el archivo que va a leer el próximo que
   agarre el proyecto.

8. **El comentario de `app.js:86` dice `261 = Mendoza`** mientras que la
   dirección del mismo archivo es de Río Negro. El dato del cliente anterior es
   internamente inconsistente; conviene no copiarlo como referencia.

9. **El comentario de `favicon.svg:3` nombra `"Allen"`** además de la letra. El
   marcador está, pero el checklist §8 sólo habla de cambiar la letra dentro del
   `<text>`, no de limpiar el comentario.

10. **`PERSONALIZAR.md` en sí mismo** conserva `Allen, Río Negro` (línea 66) e
    `IPHONE ALLEN` (línea 202) como ejemplos. Es intencional, pero es material
    del cliente anterior que queda en el repo.

### Estructuras que el checklist da por existentes y no están

11. **No existe `og:site_name` en ninguna página.** Tampoco `application-name`
    ni `theme-color`. Si se busca esa etiqueta para poner el nombre nuevo, no se
    va a encontrar: hay que crearla.

12. **No hay etiquetas `alt` escritas a mano en los HTML.** Los `alt` se generan
    en `app.js` desde `p.nombre`, así que no hay nada que personalizar ahí.

13. **No existe una imagen de Open Graph propia.** §4 pide reemplazar `og:image`
    por una de 1200 × 630 px, pero en `img/` no hay ningún archivo con esa
    finalidad: hoy las seis páginas apuntan a fotos cuadradas de producto.

### Sin discrepancia (verificado y coincide)

- 21 marcadores en el código, tal como se esperaba.
- 15 productos y 3 combos en `productos.json` (§5).
- 12 fotos en `img/` (§6).
- 7 preguntas en `FAQ`, 3 formas de pago, 3 tarjetas (§7).
- `noindex, nofollow` presente en las 6 páginas (§9).
- `CONTACTO.email` en `'[CORREO]'`, sin correo real del cliente anterior (§2).
- La dirección efectivamente está escrita en dos lugares: `app.js:99` y
  `index.html:152` (§3).
