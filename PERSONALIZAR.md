# PERSONALIZAR — checklist para el cliente nuevo

Este proyecto es una **copia de la tienda de otro cliente**. Todos los datos que
se ven (nombre, dirección, catálogo, fotos) son del negocio anterior y quedaron
puestos **a propósito**: sirven de ejemplo del formato que espera cada campo.

Esta lista es para ir tachando. Al terminar todo, el sitio queda listo para
publicar.

> **Atajo:** en el código, todo lo que hay que cambiar está marcado con el texto
> `>>> PERSONALIZAR <<<`. Buscá esa cadena en `app.js`, en los HTML, en
> `styles.css`, en `favicon.svg` y en `README.md` y vas a caer justo en cada
> lugar. Hoy son **44 marcadores**, numerados del `[1]` al `[44]`. Los que van
> del `[22]` en adelante son los **puntos ciegos**: datos del negocio anterior
> que estaban en el código sin ningún cartel al lado (ver más abajo).

---

## Las tres caras de la palabra Allen

**Antes de reemplazar nada, leer esto.**

La palabra "Allen" aparece en el proyecto con **tres significados distintos**, y
cada uno se reemplaza por un valor **diferente**:

| Cara | Cómo se ve | Qué es | Por qué valor se reemplaza |
| --- | --- | --- | --- |
| **Marca** | `IPHONE ALLEN` | El nombre del comercio anterior | Por el nombre del comercio **nuevo** |
| **Localidad** | `Allen`, `Allen, Río Negro` | La ciudad y la provincia donde estaba el local | Por la localidad **de Mendoza** del comercio nuevo |
| **Slug de dominio** | `iphone-allen`, `iphone.allen` | El identificador en la URL de Vercel y el usuario de Instagram | Por el **dominio nuevo** y el **usuario de Instagram nuevo**, que no tienen por qué parecerse al nombre |

Ejemplos concretos de las tres conviviendo en una misma línea:

```html
<title>iPhones — IPHONE ALLEN | Allen, Río Negro</title>
                  ↑ marca        ↑ localidad
<meta property="og:url" content="https://iphone-allen.vercel.app/iphones.html">
                                         ↑ slug de dominio
```

### PROHIBIDO hacer find & replace global de "Allen"

Reemplazar la palabra de una sola pasada **rompe el sitio**. Tres razones:

1. **Los tres valores nuevos son distintos entre sí.** Un reemplazo global pone
   el mismo texto en los tres lugares: el dominio queda con el nombre del
   comercio, o la localidad queda con la marca.
2. **El slug no admite el mismo formato.** `iphone-allen` va en minúscula y con
   guión, `iphone.allen` con punto, y `IPHONE ALLEN` en mayúsculas con espacio.
   Un reemplazo insensible a mayúsculas los destroza a los tres.
3. **El dominio no se cambia editando archivos.** `iphone-allen.vercel.app` sale
   del nombre del repositorio en GitHub. Cambiarlo en el código sin renombrar el
   repo deja las etiquetas Open Graph apuntando a una URL que no existe.

**El orden correcto** es ir marcador por marcador, decidiendo en cada uno cuál
de las tres caras es. Los marcadores dicen cuál: los que hablan de "negocio
anterior" son la marca, los que dicen "LOCALIDAD" son la ciudad, y los que dicen
"dominio" son el slug.

---

## Cambio de provincia

**El comercio nuevo es de MENDOZA.** El anterior era de **Allen, Río Negro**.
No alcanza con cambiar el nombre de la ciudad: hay texto de cobertura, de
envíos y de retiro que fue escrito pensando en el Alto Valle rionegrino.

> **Excepción importante:** el comentario de `app.js` línea 92
> (`54 = Argentina, 9 = celular, 261 = Mendoza`) **es correcto y no se toca**.
> La característica 261 ya es la de Mendoza. Lo que sí hay que cambiar es el
> **número** de la línea 93, que sigue siendo el del comercio anterior.

### Menciones explícitas de "Río Negro" y de la localidad

| Archivo | Línea | Dónde | Qué dice | Se ve en pantalla |
| --- | --- | --- | --- | --- |
| `index.html` | 64 | `<title>` | `... en Allen, Río Negro` | Pestaña / Google |
| `index.html` | 65 | `meta description` | `... en Allen, Río Negro ...` | Google |
| `index.html` | 83 | `og:title` | `... en Allen, Río Negro` | Al compartir |
| `index.html` | 84 | `og:description` | `Local en Allen, Río Negro ...` | Al compartir |
| `index.html` | 133 | `.eyebrow` del hero | `Allen, Río Negro · Envíos a ciudades vecinas` | **SÍ** |
| `index.html` | 161 | `.hero__direccion` | `Río Cuarto 2341, Allen, Río Negro` | **SÍ** |
| `iphones.html` | 64, 65, 83, 84 | title / description / og | `Allen, Río Negro` | Pestaña, Google, compartir |
| `mac.html` | 64, 65, 83, 84 | ídem | `Allen, Río Negro` | ídem |
| `ipad.html` | 64, 65, 83, 84 | ídem | `Allen, Río Negro` | ídem |
| `accesorios.html` | 64, 65, 83, 84 | ídem | `Allen, Río Negro` | ídem |
| `productos.html` | 64, 65, 83, 84 | ídem | `Allen, Río Negro` | ídem |
| `app.js` | 105 | constante `DIRECCION` | `Río Cuarto 2341, Allen, Río Negro` | **SÍ** (pie de página) |
| `app.js` | 224 | `FAQ`, envíos | `Hacemos envíos a Allen y ciudades vecinas` | **SÍ** |
| `favicon.svg` | 6 | comentario | `negocio ANTERIOR ("Allen")` | No |
| `README.md` | 9 | primer párrafo | `... en Allen, Río Negro` | No (pero el repo es público) |
| `README.md` | 53 | punto 4 | `"Río Cuarto 2341, Allen, Río Negro"` | No (ídem) |

### Referencias geográficas SIN la palabra "Allen"

Estas no aparecen buscando "Allen" ni "Río Negro". Son las que quedan vivas
después de un reemplazo por nombre, y todas **se ven en pantalla**:

| Archivo | Línea | Qué dice | Por qué hay que revisarla |
| --- | --- | --- | --- |
| `app.js` | **1155** | `placeholder="Ej: 2985551234"` del campo teléfono del checkout | **298 es la característica de General Roca / Allen, Río Negro.** Para Mendoza corresponde **261**. Es el ejemplo que ve todo el que completa el formulario de entrega |
| `app.js` | 1043 | `htmlFooter()`: `· Envíos a ciudades vecinas` | Texto fijo, no sale de ninguna constante. "Ciudades vecinas" en el Alto Valle no es lo mismo que en el Gran Mendoza: definir la zona real |
| `app.js` | 225 | `FAQ`, retiro: `podés retirar tu pedido en el local` | Hay que confirmar que exista local a la calle en Mendoza, y con qué horarios |
| `app.js` | 200 | `FORMAS_PAGO`, Efectivo: `En el local o contra entrega.` | Mismo caso: depende de si hay local y de hasta dónde llega la entrega |
| `index.html` | 65, 84 y los otros 5 HTML | `envíos a ciudades vecinas` | Va en las 12 metaetiquetas de las seis páginas |
| `app.js` | 93 | `WHATSAPP = '5492613900039'` | El número es del comercio anterior. Su característica (261) ya es de Mendoza, así que **no salta a la vista que está mal**: parece correcto y no lo es |

### Orden sugerido

1. Definir con el cliente: localidad exacta, dirección, zona de envíos real y si
   hay local a la calle.
2. Cambiar `DIRECCION` (`app.js` 105) y `.hero__direccion` (`index.html` 161) —
   los dos, o quedan distintos.
3. Cambiar el eyebrow (`index.html` 133).
4. Las 24 metaetiquetas geográficas de las seis páginas (líneas 64, 65, 83, 84).
5. Los textos de cobertura: FAQ envíos (224), FAQ retiro (225), footer (1043),
   formas de pago (200).
6. El `placeholder` del teléfono (1155): cambiar 298 por 261.
7. El WhatsApp (93).

---

## 1. Identidad del negocio

- [ ] **Nombre del negocio** — `app.js`, constante `NEGOCIO`, **línea 88**
      (marcador `[11]`). Es el que aparece en **todos los mensajes de WhatsApp**
      y en el **logotipo** (header y footer).
      *Ojo:* si tiene **dos palabras**, el logotipo las separa con una línea al
      medio (la 1ra liviana, la 2da en negrita). Si es **una sola palabra**, se
      muestra entera. Se adapta solo, no hay que tocar CSS.

- [ ] **Nombre en las SEIS páginas HTML** — `index.html`, `iphones.html`,
      `mac.html`, `ipad.html`, `accesorios.html`, `productos.html`.
      El nombre está en **tres etiquetas** por página:
      - `<title>` (línea 64)
      - `<meta name="description">` (línea 65)
      - `<meta property="og:title">` (línea 83)

      **`og:description` (línea 84) NO lleva la marca: lleva la LOCALIDAD.**
      Igual hay que editarla en las seis páginas, pero por el cambio de
      provincia, no por el nombre. Si se busca el nombre del negocio en esa
      etiqueta no se lo va a encontrar, y es fácil darla por revisada.

      **`index.html` tiene una cuarta ocurrencia**, fuera del `<head>`: el
      comentario `CÓMO PERSONALIZAR ESTA PÁGINA` (línea 100, marcador `[34]`).

- [ ] **Logotipo — variante del separador** (opcional) — `app.js`, constante
      `LOGO_VARIANTE`. Sólo aplica si el nombre tiene dos palabras:
      `'regla'` (línea vertical, actual), `'punto'` o `'apilado'`.

---

## 2. Contacto

- [ ] **WhatsApp** — `app.js`, constante `WHATSAPP`.
      Formato internacional, **sin +, sin espacios y sin guiones**.
      Ejemplo: `5492613900039` → `54` país, `9` celular, `261` característica.
      Es por donde entran **todos** los pedidos: si queda mal, el sitio no vende.

- [ ] **Instagram** — `app.js`, objeto `CONTACTO`, campo `instagram`.
      Va **sólo el usuario**, sin `@` y sin la URL. Hoy tiene la cuenta del
      negocio anterior.
      Si el cliente nuevo no tiene Instagram, poner `'[INSTAGRAM]'` y el botón
      desaparece solo.

- [ ] **Correo** — `app.js`, objeto `CONTACTO`, campo `email`.
      Hoy está en `'[CORREO]'` (placeholder). **No se muestra en ninguna parte**
      todavía; queda como dato del negocio.

---

## 3. Dirección y localidad

La dirección está escrita en **dos lugares distintos** y hay que cambiar los dos,
o van a quedar diferentes entre el hero y el pie de página.

- [ ] **Dirección (pie de página)** — `app.js`, constante `DIRECCION`.
- [ ] **Dirección (hero)** — `index.html`, párrafo con clase `.hero__direccion`.
- [ ] **Localidad (eyebrow del hero)** — `index.html`, párrafo con clase
      `.eyebrow`. Hoy dice `Allen, Río Negro · Envíos a ciudades vecinas`.
- [ ] **Localidad en la meta description** de las **seis** páginas HTML.
- [ ] **Zona de envíos en el FAQ** — `app.js`, array `FAQ`, la pregunta
      *"¿Hacen envíos? ¿A qué zonas?"* nombra la localidad vieja.

---

## 4. URL del sitio

Tiene que ser el dominio **real** donde quede publicado (el de Vercel o uno propio).

- [ ] **`app.js`, constante `SITIO`** — se usa para armar el link que se comparte
      desde el modal de producto.
- [ ] **`og:url` en las seis páginas** — cada página tiene la suya
      (`.../iphones.html`, `.../mac.html`, etc.), no todas la misma.
- [ ] **`og:image` en las seis páginas** — hoy apuntan a una foto de producto de
      `img/`. Lo ideal es reemplazarla por una imagen propia de **1200 × 630 px**,
      que es el formato que esperan WhatsApp y las redes.

---

## 5. Catálogo

- [ ] **Productos** — `productos.json`, array `productos`.
      Hoy hay **15 productos de ejemplo con precios inventados**. Hay que revisar
      uno por uno: `nombre`, `precio`, `stock`, `specs` y `detalle`.
      El formato completo de cada campo está explicado arriba de todo en `app.js`
      y en el `README.md`.

- [ ] **Productos usados** (si el cliente vende usados) — mismo archivo.
      Llevan `"condicion": "usado"`, `"anio"` (para ordenarlos del más nuevo al
      más viejo), el objeto `"estado"` con el informe del equipo, y
      `"equivaleNuevo"` con el id del mismo modelo nuevo para mostrar el ahorro.
      Si el cliente **no** vende usados, alcanza con que ningún producto tenga
      `"condicion": "usado"`: la sección desaparece sola.

- [ ] **Combos** — `productos.json`, array `combos`.
      Hoy hay **3 combos de ejemplo**. Cada uno son dos ids del catálogo más un
      `descuento` en porcentaje. **El precio no se escribe: se calcula solo.**
      Si el cliente no arma combos, se puede dejar el array vacío: `"combos": []`.
      *Regla:* un combo **no puede** incluir un producto usado; si lo incluye, se
      ignora y avisa por consola.

- [ ] **Categorías** — si el cliente vende otras categorías, revisar
      `ORDEN_CATEGORIAS` y `PAGINA_DE_CATEGORIA` en `app.js`. Cada categoría
      necesita su propia página HTML.

---

## 6. Fotos

- [ ] **Reemplazar las 12 fotos de `img/`** por las del cliente nuevo.
      **Convención:** el nombre del archivo tiene que ser **igual al `id` del
      producto**, más `.jpg`.
      Ejemplo: el producto con `"id": "iphone-15-128"` busca `img/iphone-15-128.jpg`.
      No hay que tocar `productos.json` para que la encuentre.

- [ ] **Tamaño:** todas cuadradas de **1000 × 1000 px**, con el producto centrado
      y fondo blanco rellenando lo que sobre. Así el recuadro mide siempre lo
      mismo en la grilla.

- [ ] Si un producto **no** tiene foto, se ve un cuadro gris con el nombre
      adentro. Es el diseño previsto para ese caso, no un error.

- [ ] **Dos productos que comparten foto:** en vez de duplicar el archivo, se usa
      el campo `"imagen"` apuntando a la ruta del otro
      (ej: `"imagen": "img/iphone-14-128.jpg"`). Ese campo le gana a la convención
      automática.

---

## 7. Textos comerciales

- [ ] **Preguntas frecuentes** — `app.js`, array `FAQ`. Son 7 preguntas con
      respuestas provisorias del tipo *"Consultanos por…"*.
      **Ojo con dos:** la de **garantía** (hoy no promete nada a propósito) y la
      de **envíos** (nombra la localidad vieja).

- [ ] **Formas de pago** — `app.js`, array `FORMAS_PAGO`. Tres tarjetas
      (Efectivo, Transferencia, Tarjeta) con un `detalle` provisorio que **no**
      menciona cuotas, recargos ni descuentos. Completar con las condiciones
      reales.

- [ ] **Promo bancaria — las cuotas** — `index.html`, sección `#promo`, el
      párrafo `.promo__titulo`. Hoy dice *"3, 6 y 12 cuotas sin interés"*.
      **Es un ejemplo:** depende de lo que le ofrezca el banco a cada comercio.

- [ ] **Promo bancaria — las tarjetas** — `app.js`, array `TARJETAS`.
      Hoy: `['Visa', 'Mastercard', 'Cabal']`.
      **Importante:** se muestran con un **ícono genérico + el nombre en texto**.
      No se usan los logos oficiales porque son **marcas registradas**. Si algún
      día se quieren los logos reales, hay que pedir autorización a cada marca.

- [ ] **Entrega estimada** — `app.js`, constante `ENTREGA_ESTIMADA`
      (hoy `'12 a 36 hs'`) y `ENTREGA_NOTA` (hoy `'Coordinamos por WhatsApp'`).
      Se editan **una sola vez** y cambian en el modal de cada producto y en el
      resumen de finalizar compra.
      Está redactado como **estimado** a propósito: conviene no convertirlo en un
      plazo prometido.

- [ ] **Textos del hero** — `index.html`, marcados con `data-editable`:
      el título (*"iPhones con soporte real"*), la bajada y los tres ítems de la
      lista `.datos` (*Liberados · Originales · Buenas condiciones*).

---

## 8. Favicons (el ícono de la pestaña)

Hoy son una **"A"** — la inicial del negocio anterior.

- [ ] **`favicon.svg`** — cambiar la letra dentro de la etiqueta `<text>`.
      Los colores salen de la paleta del sitio: fondo `#111418`, letra y marco en
      jade `#5DCAA5`.
- [ ] **`favicon-32.png`** (32 × 32) — regenerar a partir del SVG.
- [ ] **`apple-touch-icon.png`** (180 × 180) — regenerar a partir del SVG.

*Referencia de peso:* los tres juntos pesan menos de 6 KB. Si alguno queda en
cientos de KB, algo se exportó mal.

---

## Los puntos ciegos: marcadores 22 al 44

Los marcadores `[1]` a `[21]` son los que ya estaban. Los que siguen se
agregaron después de rastrear el proyecto archivo por archivo: son lugares con
datos del negocio anterior que **no tenían ningún cartel al lado**. Casi todos
son comentarios, pero dos de ellos (el bloqueo de Google y el dominio) afectan
al sitio publicado.

### El más urgente

- [ ] **`[22]` a `[27]` — el bloqueo para buscadores.** Línea 71 de las **seis**
      páginas: `<meta name="robots" content="noindex, nofollow">`.
      **Si se publica con esa línea, el sitio funciona perfecto pero no aparece
      en Google.** No tenía marcador y es lo único de esta lista que se nota
      recién cuando el cliente pregunta por qué no lo encuentra nadie.

      | # | Archivo | Línea |
      | --- | --- | --- |
      | 22 | `index.html` | 71 |
      | 23 | `iphones.html` | 71 |
      | 24 | `mac.html` | 71 |
      | 25 | `ipad.html` | 71 |
      | 26 | `accesorios.html` | 71 |
      | 27 | `productos.html` | 71 |

### El dominio viejo escrito en un comentario

- [ ] **`[28]` a `[33]` — `iphone-allen.vercel.app` dentro del comentario de
      Open Graph.** Línea 76 de las seis páginas. El marcador de la línea 52
      cubre las etiquetas, pero el dominio también está escrito en el texto del
      comentario, tres líneas más arriba de `og:image` y `og:url`.

      | # | Archivo | Línea |
      | --- | --- | --- |
      | 28 | `index.html` | 76 |
      | 29 | `iphones.html` | 76 |
      | 30 | `mac.html` | 76 |
      | 31 | `ipad.html` | 76 |
      | 32 | `accesorios.html` | 76 |
      | 33 | `productos.html` | 76 |

### El resto

| # | Archivo | Línea | Qué dato pide |
| --- | --- | --- | --- |
| 34 | `index.html` | 100 | Nombre del negocio dentro del comentario `CÓMO PERSONALIZAR ESTA PÁGINA`. Es la 4ta ocurrencia de la marca en el archivo y la única fuera del `<head>` |
| 35 | `app.js` | 8 | Nombre del negocio en el encabezado del archivo, arriba de todo |
| 36 | `app.js` | 224 | Zona de envíos del `FAQ`: nombra la localidad anterior. Queda 13 líneas debajo del marcador `[19]` del array |
| 37 | `app.js` | 3448 | Nombre del negocio en el comentario del contador de la pestaña. Es la mención más lejana al bloque de configuración |
| 38 | `styles.css` | 7 | Nombre del negocio en el encabezado de la hoja de estilos. **Es la única línea del archivo que se toca** |
| 39 | `favicon.svg` | 6 | La localidad anterior nombrada en el comentario, además de la letra del ícono |
| 40 | `README.md` | 4 | Nombre del negocio en el título del README. **El repo es público** |
| 41 | `README.md` | 9 | Localidad y provincia anteriores en el primer párrafo |
| 42 | `README.md` | 53 | Dirección completa del negocio anterior |
| 43 | `README.md` | 80 | Dominio viejo, con el slug `iphone-allen` |
| 44 | `README.md` | 124 | Nombre del negocio en el ejemplo del contador de la pestaña |

### Lo que NO existe (no perder tiempo buscándolo)

- **No hay `alt` escritos a mano** en los HTML: los genera `app.js` a partir del
  nombre de cada producto.
- **El header y el footer no están en el HTML.** El logotipo lo arma `app.js`
  desde la constante `NEGOCIO`: se cambia en un solo lugar.

---

## 9. Antes de publicar

- [ ] **Sacar el bloqueo para buscadores.** En las **seis** páginas, dentro del
      `<head>`:

      ```html
      <meta name="robots" content="noindex, nofollow">
      ```

      Está puesto a propósito para que Google **no** indexe el sitio mientras
      tiene textos de ejemplo. **Si se publica con esa línea, el sitio funciona
      perfecto pero no aparece en Google.**

- [ ] Revisar que **no quede ninguna mención del negocio anterior**. Con el
      proyecto abierto, buscar en todos los archivos:
      `Allen`, `Río Negro`, `IPHONE ALLEN`, `iphone-allen`, `iphone.allen`.
      **Buscar, sí. Reemplazar de una pasada, no:** ver
      *Las tres caras de la palabra Allen*, más arriba.

- [ ] Revisar también las referencias geográficas que **no** contienen la
      palabra "Allen" (el `placeholder` del teléfono con característica 298, los
      "envíos a ciudades vecinas", el retiro en el local): están listadas en
      *Cambio de provincia*.

- [ ] Buscar `>>> PERSONALIZAR <<<` y confirmar que ya se revisaron los **44**.

- [ ] **Renombrar el repositorio en GitHub.** El nombre actual contiene "allen"
      y es de donde Vercel saca el subdominio. No se arregla editando archivos:
      hasta que no se renombre, `SITIO` y las etiquetas `og:url` / `og:image` no
      pueden apuntar al dominio definitivo.

- [ ] Probar el **flujo completo de compra**: agregar productos al carrito,
      finalizar compra, completar el formulario y verificar que el WhatsApp se
      abre con el nombre y los datos correctos.

---

## Cómo probar el sitio mientras se edita

**No alcanza con hacer doble clic en `index.html`**: el catálogo se carga con
`fetch` y el navegador lo bloquea si el archivo se abre desde el disco
(`file://`). Hay que levantar un servidor local:

```
python -m http.server 8123
```

Y entrar a `http://localhost:8123`. Para cortarlo, `Ctrl+C`.

---

## Lo que NO hay que tocar

Salvo que se quiera cambiar el diseño o la lógica:

- `styles.css` — **salvo la línea 7.** Ahí, en el comentario del encabezado,
  está escrito el nombre del negocio anterior (marcador `[38]`). Es la única
  línea del archivo que se toca; el resto de la hoja de estilos no se modifica.
  Antes esta lista decía "la hoja de estilos completa", y por eso nadie abría
  el archivo y el nombre viejo se quedaba adentro.
- El resto de `app.js` (todo lo que no esté marcado con `>>> PERSONALIZAR <<<`):
  carrito, filtros, comparador, modal, checkout, accesibilidad.
- `README.md` — documentación técnica de cómo funciona todo.

Hay **cuatro arreglos históricos** documentados en el código que no conviene
revertir sin leer el comentario que los explica (scroll trabado sobre las
imágenes, botones tapados por el enlace de la tarjeta, `[hidden]` con
`!important`, y el filtro del foco atrapado).
