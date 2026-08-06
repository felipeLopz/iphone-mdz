# Inventario visual de `img/`

Relevamiento en **modo solo lectura**: se abrió y miró cada archivo, se midieron
dimensiones y peso, y se cruzó con `productos.json`. No se modificó ninguna
imagen, ni el JSON, ni el código.

**12 archivos, todos JPEG, todos 1000 × 1000 px, 549 KB en total.**

Todas son **renders de producto sobre fondo blanco liso**, del estilo de las
fotos de prensa de Apple. Ninguna es ambientada (no hay manos, escritorios ni
contexto), **ninguna tiene marca de agua ni texto encima**, y ninguna es logo,
favicon ni imagen de Open Graph propia.

---

## Tabla completa

| # | Archivo | Formato | Medidas | Peso | Qué se ve | Producto que la usa |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | `airpods-pro-2.jpg` | JPEG | 1000×1000 | 22,5 KB | **AirPods Pro 2ª gen.** Estuche de carga blanco abierto, los dos auriculares adentro, LED verde encendido al frente. Fondo blanco liso | `airpods-pro-2` |
| 2 | `apple-watch-s10-42.jpg` | JPEG | 1000×1000 | 39,9 KB | **Apple Watch Series 10.** Caja negra con malla deportiva negra, correa desplegada a la derecha. Esfera con "10:09" en números grandes. Fondo blanco liso | `apple-watch-s10-42` |
| 3 | `ipad-10-64.jpg` | JPEG | 1000×1000 | 41,2 KB | **iPad 10ª gen, AMARILLO.** Frente con la trasera asomando a la izquierda. Wallpaper de elipses amarillo, blanco, azul y rosa. Fondo blanco liso | `ipad-10-64` |
| 4 | `ipad-air-m2-11.jpg` | JPEG | 1000×1000 | 74,9 KB | **iPad Air M2, BLANCO ESTELAR** (beige claro). Frente + trasera. Wallpaper naranja y coral con la forma de "M". Fondo blanco liso | `ipad-air-m2-11` |
| 5 | `iphone-13-128.jpg` | JPEG | 1000×1000 | 33,4 KB | **iPhone 13, MEDIANOCHE (negro).** Trasera + frente, doble cámara en diagonal, notch. Wallpaper magenta y azul en diagonal. Fondo blanco liso | `iphone-13-128` y `iphone-13-128-usado` |
| 6 | `iphone-14-128.jpg` | JPEG | 1000×1000 | 37,2 KB | **iPhone 14, PÚRPURA/LILA.** Trasera + frente, doble cámara en diagonal, notch. Wallpaper violeta y verde. Fondo blanco liso | `iphone-14-128` y `iphone-14-128-usado` |
| 7 | `iphone-15-128.jpg` | JPEG | 1000×1000 | 43,1 KB | **Un iPhone Pro negro/titanio**: isla de TRES cámaras y Dynamic Island. Wallpaper de la onda de titanio. Fondo blanco liso. Se ve más blando de detalle que el resto | `iphone-15-128` |
| 8 | `iphone-15-pro-256.jpg` | JPEG | 1000×1000 | 101,8 KB | **iPhone 15 Pro, TITANIO AZUL.** Trasera + frente, triple cámara, Dynamic Island, wallpaper de la onda de titanio azul. Fondo blanco con un marco gris muy claro | `iphone-15-pro-256` |
| 9 | `iphone-16-pro-max-256.jpg` | JPEG | 1000×1000 | 36,2 KB | **iPhone 16 Pro, TITANIO NEGRO.** Trasera + frente, triple cámara con el botón de control de cámara, Dynamic Island. Wallpaper oscuro con elipses violeta y azul. Fondo blanco liso | `iphone-16-pro-max-256` |
| 10 | `macbook-air-m3-13.jpg` | JPEG | 1000×1000 | 62,1 KB | **MacBook Air 13", PLATA.** Abierta, de frente. Wallpaper turquesa de "plumas". Fondo blanco liso | `macbook-air-m3-13` y `macbook-air-m3-13-usado` |
| 11 | `macbook-pro-14-m4.jpg` | JPEG | 1000×1000 | 37,3 KB | **MacBook Pro 14", NEGRO ESPACIAL.** Abierta, de frente. Wallpaper negro con las curvas del chip M. Fondo blanco liso | `macbook-pro-14-m4` |
| 12 | `magsafe-15w.jpg` | JPEG | 1000×1000 | 20,1 KB | **Cargador MagSafe.** Disco blanco con aro plateado, cable saliendo hacia abajo y conector **USB-C** a la derecha. Fondo blanco liso | `magsafe-15w` |

Los 12 archivos tienen hash distinto: **no hay duplicados** en la carpeta.

---

## Cruce con `productos.json`

**Los 15 productos tienen foto.** Ninguno cae en el recuadro gris de "sin foto",
y **no sobra ningún archivo**: los 12 se usan.

**12 productos tienen foto propia** (por la convención `img/<id>.jpg`).

**3 productos NO tienen foto propia** — son los tres usados, que reutilizan la
del modelo nuevo mediante el campo `imagen`:

| Producto sin foto propia | Foto que reutiliza |
| --- | --- |
| `iphone-14-128-usado` | `img/iphone-14-128.jpg` |
| `iphone-13-128-usado` | `img/iphone-13-128.jpg` |
| `macbook-air-m3-13-usado` | `img/macbook-air-m3-13.jpg` |

Es lo correcto: no se duplica el archivo, se apunta al que ya existe.

---

## Fotos de galería (sufijos `-2` a `-5`)

**No hay ninguna.** Los 15 productos tienen una sola foto, así que el modal
nunca muestra el control de galería.

**Ojo con una trampa de nombres:** `airpods-pro-2.jpg` *parece* una foto de
galería, pero no lo es — es la foto principal del producto cuyo `id` es
`airpods-pro-2`. Hoy no hay conflicto porque no existe ningún producto con id
`airpods-pro`. **Pero si algún día se carga uno**, el código va a leer
`airpods-pro-2.jpg` como su segunda foto de galería y la va a mostrar dentro de
la ficha del otro producto. Conviene no crear ids que terminen en `-2` a `-5`.

---

## Logo, favicon y Open Graph

**En `img/` no hay ninguno**: los 12 archivos son fotos de producto.

Los íconos de marca viven en la **raíz** del proyecto, no acá:

| Archivo | Uso |
| --- | --- |
| `favicon.svg` | Favicon principal (la letra del negocio) |
| `favicon-32.png` | Favicon 32×32 para navegadores sin SVG |
| `apple-touch-icon.png` | Ícono 180×180 de "agregar a pantalla de inicio" |

**Cuatro fotos de producto hacen doble función como imagen de Open Graph** (la
vista previa al compartir el link):

| Foto | Páginas que la usan como `og:image` |
| --- | --- |
| `iphone-16-pro-max-256.jpg` | `index.html`, `iphones.html`, `productos.html` |
| `macbook-air-m3-13.jpg` | `mac.html` |
| `ipad-air-m2-11.jpg` | `ipad.html` |
| `airpods-pro-2.jpg` | `accesorios.html` |

**No es lo ideal:** WhatsApp y las redes esperan una imagen apaisada de
**1200 × 630 px**, y estas son cuadradas de 1000 × 1000, así que el recorte lo
decide cada plataforma. Falta una imagen propia con ese formato.

---

## Resolución: ¿alguna por debajo de 800 px?

**Ninguna.** Las 12 miden exactamente 1000 × 1000 px, que es la medida objetivo.

Con una advertencia: el `README.md` documenta que **tres de estas fotos venían
más chicas y se ampliaron** para unificar la medida — el iPhone 15 a 300 px, el
iPhone 13 a 500 px y la MacBook Air a 900 px. Ampliar no inventa detalle: miden
1000 px pero no tienen la nitidez de 1000 px reales.

Mirándolas, `iphone-15-128.jpg` y `iphone-13-128.jpg` se ven notoriamente más
blandas que el resto, lo cual es consistente con esa nota. **Esas dos conviene
reemplazarlas** por fotos en resolución nativa cuando se pueda. Es una impresión
visual, no una medición: el archivo no guarda su historial de escalado.

---

## Coherencia entre la foto y los datos del producto

Al cruzar el color que se ve en cada foto contra el que declara
`productos.json`, **7 de 12 no coinciden**:

| Foto | Color en la foto | Color en `productos.json` | |
| --- | --- | --- | --- |
| `iphone-16-pro-max-256.jpg` | Titanio **negro** | Titanio **natural** | ❌ |
| `iphone-15-pro-256.jpg` | Titanio azul | Titanio azul | ✅ |
| `iphone-15-128.jpg` | Un **Pro** negro, con **3 cámaras** | iPhone 15 a secas, **Negro** (lleva 2 cámaras) | ❌ **modelo equivocado** |
| `iphone-14-128.jpg` | **Púrpura** | **Azul medianoche** | ❌ |
| `iphone-13-128.jpg` | **Medianoche** (negro) | **Blanco estelar** | ❌ |
| `macbook-air-m3-13.jpg` | **Plata** | **Medianoche** | ❌ |
| `macbook-pro-14-m4.jpg` | Negro espacial | Negro espacial | ✅ |
| `ipad-air-m2-11.jpg` | **Blanco estelar** | **Gris espacial** | ❌ |
| `ipad-10-64.jpg` | **Amarillo** | **Plata** | ❌ |
| `airpods-pro-2.jpg` | Blanco | (no declara color) | ✅ |
| `apple-watch-s10-42.jpg` | Negro | "Aluminio", sin color | ➖ |
| `magsafe-15w.jpg` | Blanco | (no declara color) | ✅ |

El caso más serio es `iphone-15-128.jpg`: no es sólo un color distinto, **es otro
modelo**. La foto muestra un iPhone Pro con isla de tres cámaras y acabado de
titanio; el producto es un iPhone 15 común, que tiene dos cámaras en diagonal y
carcasa de aluminio.

Nada de esto rompe el sitio y es esperable —los datos son del catálogo de ejemplo
del comercio anterior— pero **hay que rehacer el cruce cuando entren los
productos reales**: la foto y el campo `Color` tienen que decir lo mismo, o el
cliente que compra "Azul medianoche" recibe algo que no vio.

---

## Agrupación para un catálogo de iPhones + accesorios + baterías/carga

| Grupo | Fotos utilizables | Cuáles |
| --- | --- | --- |
| **iPhone** | **5** | `iphone-16-pro-max-256` · `iphone-15-pro-256` · `iphone-15-128` · `iphone-14-128` · `iphone-13-128` |
| **Accesorios** | **2** | `airpods-pro-2` (auriculares) · `apple-watch-s10-42` (reloj) |
| **Baterías / carga** | **1** | `magsafe-15w` (cargador inalámbrico + cable USB-C) |
| **Fuera de estos grupos** | 4 | `macbook-air-m3-13` · `macbook-pro-14-m4` · `ipad-air-m2-11` · `ipad-10-64` |

**Total directamente aprovechable para ese catálogo: 8 de 12.**

### Detalle por grupo

**iPhone — 5 fotos, y cubren una gama variada:** dos gama alta actuales (16 Pro
negro, 15 Pro azul), una intermedia (la de `iphone-15-128`, que en realidad
muestra un Pro) y dos de generaciones anteriores (14 púrpura, 13 medianoche).
Alcanzan para armar una grilla que no se vea repetida, porque los colores y las
generaciones son distintos entre sí.

**Accesorios — 2 fotos, y quedan cortas.** Cubren auriculares y relojes. Un
catálogo de accesorios de iPhone normalmente necesita además fundas, vidrios
templados, cables y soportes, y de eso no hay nada. Es el grupo con el hueco más
grande.

**Baterías y carga — 1 sola foto.** El MagSafe es lo único. No hay ninguna
imagen de power bank, de cargador de pared, de cable Lightning ni de cargador
de auto. Si ese va a ser un rubro del negocio, **hay que conseguir fotos casi
desde cero**.

### ¿Se puede reutilizar algo de Mac / iPad?

**Como foto de producto, no.** Una MacBook o un iPad no pueden ilustrar un
iPhone, un accesorio ni una batería: el cliente ve exactamente lo que va a
recibir, y poner otra cosa es engañoso.

Las dos únicas reutilizaciones que sí tienen sentido:

1. **`magsafe-15w.jpg` sirve para más de un producto del rubro carga.** Se ve el
   disco y el cable USB-C, así que puede ilustrar tanto el cargador inalámbrico
   como, recortada, un cable USB-C genérico. Es la más versátil de las 12.
2. **Las cuatro de Mac/iPad sirven si el negocio también vende Mac o iPad.** Si
   el catálogo nuevo se limita a iPhone + accesorios + carga, quedan sin uso: no
   conviene borrarlas todavía, pero tampoco cuentan como stock de fotos.

Para una imagen de portada o de Open Graph, cualquiera de las cuatro puede servir
como imagen "de ambiente" — pero para eso lo correcto es una pieza propia de
1200 × 630 px, que hoy no existe.

---

## Qué falta pedirle al cliente

- **Fotos de accesorios**: fundas, vidrios, cables, soportes. Hoy hay 2 fotos
  para todo el rubro.
- **Fotos de baterías y carga**: power banks, cargadores de pared, cables,
  cargador de auto. Hoy hay 1 sola.
- **Reemplazo de `iphone-15-128.jpg` y `iphone-13-128.jpg`** por fotos en
  resolución nativa: las actuales están ampliadas y se notan blandas.
- **Una imagen de Open Graph propia de 1200 × 630 px**, para que la vista previa
  al compartir el link no dependa del recorte de una foto cuadrada.
- Que cada foto llegue **con el nombre del código del producto**, en `.jpg`,
  cuadrada de 1000 × 1000 con fondo blanco. El detalle completo está en
  `ESQUEMA-PRODUCTOS.md`, sección "Qué pedirle al cliente".
