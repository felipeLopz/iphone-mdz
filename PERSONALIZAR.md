# PERSONALIZAR — checklist para el cliente nuevo

Este proyecto es una **copia de la tienda de otro cliente**. Todos los datos que
se ven (nombre, dirección, catálogo, fotos) son del negocio anterior y quedaron
puestos **a propósito**: sirven de ejemplo del formato que espera cada campo.

Esta lista es para ir tachando. Al terminar todo, el sitio queda listo para
publicar.

> **Atajo:** en el código, todo lo que hay que cambiar está marcado con el texto
> `>>> PERSONALIZAR <<<`. Buscá esa cadena en `app.js` y en los HTML y vas a caer
> justo en cada lugar.

---

## 1. Identidad del negocio

- [ ] **Nombre del negocio** — `app.js`, constante `NEGOCIO` (cerca de la línea 68).
      Es el que aparece en **todos los mensajes de WhatsApp** y en el **logotipo**
      (header y footer).
      *Ojo:* si tiene **dos palabras**, el logotipo las separa con una línea al
      medio (la 1ra liviana, la 2da en negrita). Si es **una sola palabra**, se
      muestra entera. Se adapta solo, no hay que tocar CSS.

- [ ] **Nombre en las SEIS páginas HTML** — `index.html`, `iphones.html`,
      `mac.html`, `ipad.html`, `accesorios.html`, `productos.html`.
      En cada una hay que cambiarlo en **cuatro lugares**:
      - `<title>`
      - `<meta name="description">`
      - `<meta property="og:title">`
      - `<meta property="og:description">`

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

- [ ] Buscar `>>> PERSONALIZAR <<<` y confirmar que ya se revisó cada uno.

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

- `styles.css` — la hoja de estilos completa.
- El resto de `app.js` (todo lo que no esté marcado con `>>> PERSONALIZAR <<<`):
  carrito, filtros, comparador, modal, checkout, accesibilidad.
- `README.md` — documentación técnica de cómo funciona todo.

Hay **cuatro arreglos históricos** documentados en el código que no conviene
revertir sin leer el comentario que los explica (scroll trabado sobre las
imágenes, botones tapados por el enlace de la tarjeta, `[hidden]` con
`!important`, y el filtro del foco atrapado).
