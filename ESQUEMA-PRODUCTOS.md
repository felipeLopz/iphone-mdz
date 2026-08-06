# ESQUEMA DE `productos.json`

Referencia técnica del catálogo: qué campo existe, de qué tipo es, si hace falta
o no, y **qué pasa exactamente si falta**. Lo de "qué pasa si falta" no está
supuesto: se probó cargando un catálogo con productos incompletos a propósito y
mirando qué renderiza el sitio y qué avisa la consola.

**Resumen del comportamiento general: nada rompe la página.** No hubo un solo
error de JavaScript en toda la prueba. Cuando falta un campo, el sitio o lo
saltea en silencio, o muestra un valor degradado (`undefined`, `$ NaN`). Eso es
cómodo pero peligroso: **un dato mal cargado no se nota hasta que alguien lo ve
en pantalla.** Por eso conviene revisar contra este documento antes de publicar.

---

## Estructura general del archivo

```json
{
  "productos": [ ... ],
  "combos":    [ ... ]
}
```

Dos listas de primer nivel. `app.js` también acepta la forma vieja (un array
suelto de productos, sin combos) para no romper un JSON anterior, pero lo que
hay que usar es la de arriba.

---

## Tabla resumen: obligatorio vs opcional

| Campo | Tipo | ¿Obligatorio? | Si falta |
| --- | --- | --- | --- |
| `id` | texto | **SÍ** | La tarjeta se dibuja, pero el producto queda roto: el botón de detalle sale con `data-modal="undefined"` y la foto se busca en `img/undefined.jpg` |
| `nombre` | texto | **SÍ** | Se imprime literalmente la palabra **`undefined`** como nombre del producto, en la tarjeta y en el modal |
| `categoria` | texto | **SÍ** | El producto igual aparece, pero cae en una sección sin nombre y no lo alcanza ningún filtro de categoría |
| `precio` | número | **SÍ** | Se muestra **`$ NaN`** en la tarjeta, en el modal y en el carrito |
| `specs` | lista de textos | **SÍ** (en la práctica) | No rompe: la línea de descripción queda vacía. La tarjeta se ve incompleta |
| `detalle` | objeto | **SÍ** (en la práctica) | No rompe: el modal queda sin ficha técnica y en el comparador todas las filas de ese producto salen "—" |
| `precioAnterior` | número | No | No se muestra el precio tachado |
| `destacado` | booleano | No | No entra al carrusel de la home |
| `etiqueta` | texto (enum) | No | No se muestra cartelito |
| `principal` | booleano | No | No es la tarjeta grande de su categoría |
| `stock` | número | No | **Se asume que hay stock** |
| `imagen` | texto | No | Se usa la convención `img/<id>.jpg` |
| `subcategoria` | texto | No | Sólo aplica a Accesorios. Sin ella, el producto va a una sección "Otros" |
| `condicion` | texto (enum) | No | El producto se considera **nuevo** |
| `anio` | número | No | El usado va al final de su sección |
| `estado` | objeto | No | No se muestra el bloque "Estado del equipo" |
| `equivaleNuevo` | texto (id) | No | No se muestra cuánto se ahorra contra el nuevo |

Los cuatro primeros (`id`, `nombre`, `categoria`, `precio`) son los que producen
basura visible si faltan. `specs` y `detalle` no rompen nada, pero un producto
sin ellos se ve pobre y no sirve para comparar.

---

## Campo por campo

### `id` — texto, OBLIGATORIO

El identificador interno. **No se muestra nunca en pantalla**, pero es la pieza
que más cosas sostiene:

- arma la ruta de la foto por convención: `img/<id>.jpg`;
- es lo que viaja en la URL al compartir un producto (`?producto=<id>`) y al
  mandarlo al comparador (`?comparar=<id>`);
- es lo que guarda el carrito en el navegador;
- es a lo que apuntan `equivaleNuevo` y la lista `productos` de un combo.

**Formato:** minúsculas, sin espacios, sin acentos ni ñ, palabras separadas con
guión medio. Tiene que poder ser un nombre de archivo válido en Windows, porque
va a serlo.

```
iphone-15-128            ✓
macbook-air-m3-13        ✓
iPhone 15 (128 GB)       ✗  espacios, mayúsculas y paréntesis
cargador-20w-ñandú       ✗  la ñ y el acento rompen la ruta de la foto
```

**Convención sugerida:** `<modelo>-<capacidad o variante>`, y para los usados el
mismo id del nuevo más `-usado` (`iphone-14-128` → `iphone-14-128-usado`).

**Tiene que ser único.** El código no valida duplicados: si hay dos productos con
el mismo id, las búsquedas devuelven siempre el primero y el segundo queda
inalcanzable — no se puede abrir, ni agregar al carrito, ni comparar.

**Si falta:** probado. La tarjeta se dibuja igual y muestra nombre y precio, pero
el botón sale con `data-modal="undefined"`, así que no abre su ficha, y la foto
se busca en `img/undefined.jpg`, que no existe.

### `nombre` — texto, OBLIGATORIO

Se muestra tal cual en la tarjeta, el carrusel, el modal, el comparador, el
carrito, el `alt` de la foto y el mensaje de WhatsApp del pedido.

**Si falta:** probado. Se imprime la palabra `undefined` en todos esos lugares.
No hay error, sólo un producto que dice "undefined".

**Recomendación para usados:** que el nombre diga `(usado)`. En el carrito y en
el WhatsApp el nuevo y el usado aparecen juntos y ahí el cartelito de la tarjeta
no está para distinguirlos.

### `categoria` — texto, OBLIGATORIO

Define en qué sección y en qué página aparece el producto.

**Valores que funcionan hoy:** `iPhone`, `Mac`, `iPad`, `Accesorios`. Están
escritos en dos lugares de `app.js`: `ORDEN_CATEGORIAS` (el orden de las
secciones) y `PAGINA_DE_CATEGORIA` (a qué archivo HTML corresponde cada una).
**Ojo con las mayúsculas: la comparación es exacta.** `iphone` no es `iPhone`.

**Categoría nueva:** probado. Una categoría que no esté en la lista (por ejemplo
`Fundas`) **funciona igual**: el producto aparece, se crea su sección al final
del catálogo y entra en los filtros. Lo que NO se crea sola es su página propia:
para eso hay que sumar la categoría a `ORDEN_CATEGORIAS` y a
`PAGINA_DE_CATEGORIA`, y crear el HTML.

**Si falta:** probado. El producto aparece igual, en una sección sin título, y
queda fuera de todos los filtros por categoría.

### `precio` — número, OBLIGATORIO

Número pelado, **sin puntos, sin comas y sin el signo `$`**. Se formatea solo en
pesos argentinos.

```
1749000      ✓
"$1.749.000" ✗
1749000.50   ✗ evitar decimales
```

**Si falta:** probado. Se muestra `$ NaN` en la tarjeta, el modal y el carrito, y
el total del pedido queda en `NaN`. Es el error más caro de los cuatro.

### `precioAnterior` — número, opcional

El precio viejo, que se muestra tachado al lado del actual. Tiene que ser **mayor
que `precio`** para que tenga sentido. Si no se pone, no se muestra nada tachado
(y el espacio queda reservado igual, para que todas las tarjetas midan lo mismo).

### `destacado` — booleano, opcional

`true` hace que el producto entre en el carrusel "Destacados de la semana" de la
home. Lo ideal son **3 o 4** en todo el catálogo. Si ningún producto lo tiene, el
carrusel se llena solo con los tres primeros del JSON.

### `etiqueta` — texto, opcional, ENUM

Cartelito arriba a la izquierda de la foto. **Sólo dos valores admitidos:**

| Valor | Qué muestra |
| --- | --- |
| `"nuevo"` | **Nuevo ingreso** (de contorno, sobrio) |
| `"oferta"` | **Oferta** (relleno, más fuerte) |

**Cualquier otro valor no muestra nada** — probado con `"rebajado"`: no aparece
cartelito y tampoco avisa por consola. Es un error silencioso: si el cliente
escribe "OFERTA" en mayúsculas, la etiqueta simplemente no sale.

El cartel de "Sin stock" va arriba a la derecha y el de "Usado" abajo a la
izquierda, así que los tres pueden convivir sin pisarse.

### `principal` — booleano, opcional

`true` marca cuál es la tarjeta grande (doble alto) de su categoría.

**Si hay más de uno en la misma categoría:** probado. Se usa el primero y avisa
por consola:

```
[tienda] la categoría "Mac" tiene 2 productos con "principal": true.
Se usa el primero (dos-principales-a). Dejá uno solo en productos.json.
```

Si no hay ninguno, se usa el primero de la lista. Y si la categoría tiene un
combo con stock, el combo ocupa esa celda y el producto principal pasa a ser una
tarjeta normal.

### `stock` — número, opcional

| Valor | Qué se ve |
| --- | --- |
| campo ausente | **Disponible** (se asume que hay) |
| `0` | Producto atenuado, cartel **Sin stock**, y el botón cambia de "Agregar" a "Avisame cuando llegue" (abre WhatsApp) |
| `1` | **¡Última unidad!** en rojo |
| `2` a `5` | **¡Últimas N unidades!** en rojo |
| `6` o más | **Disponible**, sin número |

El corte de "stock bajo" es la constante `STOCK_BAJO = 5` en `app.js`.

**Importante:** `stock: 0` **no oculta** el producto, lo deja visible y atenuado.
Para sacar un producto del catálogo hay que borrar su bloque del JSON.

### `specs` — lista de textos, obligatorio en la práctica

La descripción corta de la tarjeta. Se muestran unidos con ` · `.

**Cuántos:** de 2 a 4. El CSS recorta a dos líneas y reserva ese alto siempre,
así que pasarse no rompe nada pero se corta con puntos suspensivos.

**Si falta:** probado. No rompe: la línea queda vacía y la tarjeta se ve pelada.

**En un usado**, esta misma línea muestra el resumen del estado (batería +
pantalla) en lugar de las últimas specs, así que conviene **no repetir la batería
en `specs`** si ya está en `estado`.

### `detalle` — objeto, obligatorio en la práctica

La ficha técnica. Es un objeto de pares **clave: valor**, y se muestra en el mismo
orden en que se escriben:

```json
"detalle": {
  "Pantalla": "6.1\" Super Retina XDR",
  "Procesador": "A16 Bionic"
}
```

Aparece en **dos lugares**: la ficha del modal, y **la tabla del comparador** —
cada clave es una fila. Por eso las claves importan tanto (ver la sección
dedicada más abajo).

**Si falta:** probado. No rompe. El modal queda sin ficha técnica, y en el
comparador ese producto muestra "—" (con texto "Sin dato" para lectores de
pantalla) en todas las filas que aporta el otro.

### `imagen` — texto, opcional

Ruta explícita a la foto. **Casi nunca hace falta escribirlo.**

```js
// app.js
return p.imagen || ('img/' + p.id + '.jpg');
```

Si el campo está y no está vacío, esa ruta gana. Si no está (o está en `""`), se
arma sola: `img/<id>.jpg`.

**Cuándo sí escribirlo:** cuando dos productos comparten la misma foto, para no
duplicar el archivo. Es el caso de los usados, que apuntan a la foto del modelo
nuevo:

```json
"imagen": "img/iphone-14-128.jpg"
```

**Si la foto no existe:** no aparece el ícono roto. Un listener de `error` la
reemplaza por un recuadro gris con el nombre del producto adentro. Es el diseño
previsto, no un error.

### `subcategoria` — texto, opcional

**Sólo se usa en `Accesorios`.** iPhone, Mac e iPad se agrupan por categoría a
secas y no la llevan.

Hoy hay tres: `Auriculares`, `Relojes`, `Cargadores`. **Se derivan solas del
JSON**: escribir una subcategoría nueva en un producto la hace aparecer en el
menú y como sección, sin tocar código. Una subcategoría sin productos no se
muestra.

**Si falta en un accesorio:** probado. El producto no desaparece: se agrupa al
final, en una sección llamada **"Otros"**.

### `condicion` — texto, opcional, ENUM

**Es el campo que distingue un usado de un nuevo.** Un único valor tiene efecto:

```json
"condicion": "usado"
```

```js
// app.js
function esUsado(p) { return p.condicion === 'usado'; }
```

**Cualquier otro valor cuenta como nuevo** — probado con `"reacondicionado"`: el
producto se muestra en la sección de nuevos, sin cartelito "Usado" y sin bloque
de estado. Es una comparación exacta de string: `"Usado"` con mayúscula tampoco
funciona.

Si ningún producto del catálogo tiene `condicion: "usado"`, toda la sección de
usados desaparece sola y el sitio queda como si nunca hubiera existido.

**Un usado es una entrada propia y duplicada**, no un atributo del producto
nuevo: el mismo modelo puede estar sellado y usado a la vez, a precios distintos.

### `anio` — número, opcional (sólo usados)

El año del modelo. Sirve para ordenar la sección de usados **del más nuevo al más
viejo**.

**Si falta:** probado. El usado no desaparece: queda al final de la lista, en el
orden en que viene del JSON.

### `estado` — objeto, opcional (sólo usados)

El informe de transparencia del equipo. **Todos sus campos son opcionales**; el
que no se carga simplemente no se muestra, sin dejar una fila vacía.

| Clave | Qué se escribe | Cómo se muestra |
| --- | --- | --- |
| `bateria` | **número**, sin el `%` (ej: `87`) | "Batería — 87%" (el `%` lo agrega la página) |
| `pantalla` | texto corto | "Pantalla" |
| `carcasa` | texto corto | "Carcasa" |
| `uso` | cuánto se usó | "Tiempo de uso" |
| `reparaciones` | `"Ninguna"` o qué se cambió | "Reparaciones" |
| `accesorios` | qué viene con el equipo | "Accesorios" |

Se muestran en ese orden fijo. Una clave que no esté en la lista igual se
muestra, al final, con la primera letra en mayúscula.

**Si falta el objeto entero:** probado. No rompe. El bloque "Estado del equipo"
directamente no se dibuja (salvo que haya comparación de precio contra el nuevo,
en cuyo caso el bloque aparece sólo con eso).

### `equivaleNuevo` — texto (un id), opcional (sólo usados)

El `id` del mismo modelo **nuevo** del catálogo. Con eso la página calcula sola
cuánto se ahorra y lo muestra en la tarjeta (`Nuevo $1.399.000 · Ahorrás
$249.000`) y en el modal (con el porcentaje).

**No hay que escribir ningún monto:** si mañana cambia cualquiera de los dos
precios, la cuenta se actualiza sola.

No se muestra ninguna comparación si: el campo no está, **el id no existe**
(probado: no rompe, simplemente no aparece la línea), el id apunta a otro usado,
o el usado quedara igual o más caro que el nuevo.

---

## Los combos

**No hay ningún campo `esCombo` en `productos.json`.** Es lo primero que confunde
al leer `app.js:1980`, donde aparece `p.esCombo`.

Un combo se define en la lista `combos`, con su propio esquema:

| Campo | Tipo | Qué es |
| --- | --- | --- |
| `id` | texto | Único. Conviene que empiece con `combo-` |
| `categoria` | texto | En qué sección ocupa la tarjeta grande |
| `productos` | lista de **exactamente 2** ids | Tienen que existir en el catálogo |
| `descuento` | número | Porcentaje sobre la suma de los dos precios |
| `etiqueta` | texto | Opcional, por defecto `"combo"` |

`construirCombos()` toma cada definición y **fabrica en memoria un objeto con la
misma forma que un producto** (id, nombre, precio, stock, imagen…), agregándole
dos cosas que sólo existen ahí: la marca `esCombo: true` y la lista `items` con
los dos productos. Por eso el carrito, el modal y el WhatsApp lo tratan como un
producto más sin saber que es un combo.

Lo que el combo calcula solo, y **no se escribe**:

- `precio` = suma de los dos × (1 − descuento/100), redondeado
- `precioAnterior` = la suma sin descuento (para mostrarla tachada)
- `nombre` = los dos nombres unidos con ` + `
- `stock` = el menor de los dos (el combo se agota con el primero que se agote)
- `imagen` = la del primer producto

**Reglas que hacen que un combo se ignore** (avisa por consola y la página sigue
funcionando; ambas probadas):

```
Combo "combo-con-usado": incluye producto(s) usado(s) (usado-sin-estado).
Los combos son sólo de productos nuevos. Se ignora.

Combo "combo-id-inexistente": necesita dos ids que existan en el catálogo. Se ignora.
```

Además, el combo **no se muestra** (sin avisar) si alguno de los dos productos se
quedó sin stock, o si hay una búsqueda o un filtro de precio puestos y el combo
no entra.

Si el cliente no arma combos, se deja la lista vacía: `"combos": []`.

---

## Las claves de `detalle`: qué usa cada categoría

Esto importa porque **`detalle` es lo que alimenta la tabla del comparador**. El
comparador toma las claves del producto A, le suma las de B que A no tenía, y
arma una fila por clave. Cuando una clave existe en uno y no en el otro, la celda
que falta sale "—".

Conclusión práctica: **dos productos comparan bien sólo si comparten las claves.**

### Estado actual de los 15 productos

| Categoría | Productos | ¿Comparten claves? |
| --- | --- | --- |
| **iPhone** | 7 | ✅ **Sí, los 7 idénticos** |
| **Mac** | 3 | ✅ **Sí, los 3 idénticos** |
| **iPad** | 2 | ✅ **Sí, los 2 idénticos** |
| **Accesorios** | 3 | ❌ **No: 3 productos, 3 juegos distintos** |

**iPhone** (8 claves, las mismas en los 7):
`Pantalla · Procesador · Cámara · Batería · Almacenamiento · Color · Conector · Estado`

**Mac** (8 claves, las mismas en los 3):
`Pantalla · Procesador · Memoria · Almacenamiento · Batería · Puertos · Color · Estado`

**iPad** (8 claves, las mismas en los 2):
`Pantalla · Procesador · Almacenamiento · Conectividad · Cámara · Batería · Compatibilidad · Color`

**Accesorios** — acá está la disparidad, cada uno con su juego:

| Producto | Subcategoría | Claves |
| --- | --- | --- |
| `airpods-pro-2` | Auriculares | Chip, Audio, Espacial, Batería, Estuche, Resistencia, Incluye, Estado |
| `apple-watch-s10-42` | Relojes | Pantalla, Chip, Caja, Batería, Salud, Resistencia, Conectividad, Incluye |
| `magsafe-15w` | Cargadores | Potencia, Compatibilidad, Cable, Conector, Requiere, Origen, Estado |

Entre los tres sólo `Batería` aparece en dos de ellos; el cargador no comparte
**ninguna** clave con los otros dos. Comparar AirPods contra un cargador hoy da
una tabla de 15 filas donde casi todas tienen un "—" de un lado.

En parte es razonable —un cargador no tiene pantalla— pero conviene fijar un
juego común para que la comparación sirva de algo.

### Juego fijo propuesto para el catálogo nuevo

Para iPhone, Mac e iPad se mantiene lo que ya está (funciona). Para Accesorios
se propone este juego de 8, pensado para que cualquier accesorio pueda llenarlo:

`Tipo · Compatibilidad · Batería · Conectividad · Resistencia · Incluye · Origen · Estado`

Es el que usa `plantilla-productos.csv`.

---

## Ejemplos completos y comentados

### 1. Producto NUEVO

```json
{
  "id": "iphone-15-128",              // único, sin espacios ni acentos; define img/iphone-15-128.jpg
  "nombre": "iPhone 15 128 GB",       // se muestra tal cual
  "categoria": "iPhone",              // exacto: iPhone | Mac | iPad | Accesorios
  "precio": 1749000,                  // número pelado, sin puntos ni $
  "precioAnterior": 1899000,          // opcional: se muestra tachado. Tiene que ser mayor
  "destacado": false,                 // true => entra al carrusel de la home (que sean 3 o 4)
  "etiqueta": "oferta",               // opcional: sólo "nuevo" u "oferta"
  "principal": false,                 // true => tarjeta grande de la categoría (uno solo por categoría)
  "stock": 5,                         // sin el campo se asume que hay. 0 => "Sin stock"
  "specs": [                          // 2 a 4 datos cortos, se unen con " · "
    "128 GB",
    "Negro",
    "Batería 100%",
    "Sellado"
  ],
  "detalle": {                        // ficha del modal Y filas del comparador
    "Pantalla": "6.1\" Super Retina XDR",
    "Procesador": "A16 Bionic",
    "Cámara": "48 MP principal + 12 MP ultra gran angular",
    "Batería": "Hasta 20 h de video · salud 100%",
    "Almacenamiento": "128 GB",
    "Color": "Negro",
    "Conector": "USB-C",
    "Estado": "Sellado, liberado de fábrica"
  }
  // sin "imagen": se usa la convención img/iphone-15-128.jpg
}
```

### 2. Producto USADO

Es una entrada aparte, con su propio id. Lo que lo hace usado es **una sola
línea**: `"condicion": "usado"`.

```json
{
  "id": "iphone-14-128-usado",        // el id del nuevo + "-usado"
  "nombre": "iPhone 14 128 GB (usado)", // conviene que diga (usado)
  "categoria": "iPhone",
  "condicion": "usado",               // <-- ESTO es lo único que lo marca como usado
  "anio": 2022,                       // ordena la sección: del más nuevo al más viejo
  "equivaleNuevo": "iphone-14-128",   // id del mismo modelo NUEVO => calcula el ahorro solo
  "precio": 1150000,
  "destacado": false,
  "principal": false,
  "stock": 2,
  "specs": [                          // sin la batería: ya la pone "estado"
    "128 GB",
    "Azul medianoche",
    "Liberado"
  ],
  "estado": {                         // el informe del equipo; todos los campos opcionales
    "bateria": 92,                    // NÚMERO, sin el %
    "pantalla": "Sin rayones",
    "carcasa": "Marca leve en el borde inferior",
    "uso": "1 año y medio",
    "reparaciones": "Ninguna",
    "accesorios": "Con caja, sin cargador"
  },
  "detalle": {                        // mismas claves que los demás iPhone
    "Pantalla": "6.1\" Super Retina XDR",
    "Procesador": "A15 Bionic",
    "Cámara": "12 MP principal + 12 MP ultra gran angular",
    "Batería": "Hasta 20 h de video · salud 92%",
    "Almacenamiento": "128 GB",
    "Color": "Azul medianoche",
    "Conector": "Lightning",
    "Estado": "Usado muy bueno, liberado"
  },
  "imagen": "img/iphone-14-128.jpg"   // reusa la foto del nuevo, no se duplica el archivo
}
```

### 3. COMBO

Va en la lista `combos`, no en `productos`. **El precio no se escribe.**

```json
{
  "id": "combo-iphone-16-magsafe",    // conviene el prefijo "combo-"
  "categoria": "iPhone",              // en qué sección ocupa la tarjeta grande
  "productos": [                      // EXACTAMENTE dos ids que existan y NO sean usados
    "iphone-16-pro-max-256",
    "magsafe-15w"
  ],
  "descuento": 10,                    // porcentaje sobre la suma de los dos precios
  "etiqueta": "combo"                 // opcional, es el valor por defecto
}
```

---

## Las fotos

**Convención:** el nombre del archivo tiene que ser **igual al `id` del producto**,
más `.jpg`, dentro de `img/`.

```
"id": "iphone-15-128"   ->   img/iphone-15-128.jpg
```

No hay que tocar `productos.json` para que la encuentre.

**Medida:** cuadradas de **1000 × 1000 px**, con el producto centrado y fondo
blanco rellenando lo que sobre. Así el recuadro mide siempre lo mismo en la
grilla. Emparejar el lienzo no empareja cuánto ocupa el producto adentro: si en
una foto el equipo sale más de cerca que en otra, eso viene de la foto original
y hay que reencuadrarla.

**Formato:** `.jpg`. Es la única extensión que busca el código.

**Varias fotos del mismo producto (galería):** también por nombre de archivo,
agregando un número. Se buscan **en orden y se corta en la primera que falta**:

```
img/iphone-15-128.jpg      <- la principal
img/iphone-15-128-2.jpg
img/iphone-15-128-3.jpg
img/iphone-15-128-4.jpg
img/iphone-15-128-5.jpg
```

Máximo **5 en total**. Con una sola foto el detalle se ve como siempre, sin
controles de galería. Si están la `-2` y la `-4` pero falta la `-3`, sólo se ve
la `-2`.

**Si un producto no tiene foto:** se ve un recuadro gris con el nombre adentro.
Es el diseño previsto para ese caso, no un error.

---

## De la planilla al JSON

`plantilla-productos.csv` tiene **40 columnas**: 24 de datos del producto y 16
para la ficha técnica. Esta es la correspondencia con el JSON, para quien haga la
conversión:

| Columna del CSV | Campo del JSON | Nota |
| --- | --- | --- |
| `codigo` | `id` | tal cual |
| `nombre` | `nombre` | tal cual |
| `categoria` | `categoria` | tal cual |
| `subcategoria` | `subcategoria` | omitir si viene vacía |
| `condicion` | `condicion` | sólo si dice `usado`; si no, omitir el campo |
| `anio_del_modelo` | `anio` | a número |
| `codigo_del_mismo_modelo_nuevo` | `equivaleNuevo` | omitir si viene vacía |
| `precio` | `precio` | **a número** |
| `precio_anterior` | `precioAnterior` | a número; omitir si viene vacía |
| `stock` | `stock` | a número; **omitir si viene vacía** (vacío ≠ 0) |
| `etiqueta` | `etiqueta` | validar contra `nuevo` / `oferta` |
| `destacado` | `destacado` | `si`/`no` → `true`/`false` |
| `tarjeta_grande` | `principal` | `si`/`no` → `true`/`false` |
| `spec_1` … `spec_4` | `specs` | armar el array salteando las vacías |
| `estado_bateria` | `estado.bateria` | **a número**, sin el `%` |
| `estado_pantalla` … `estado_accesorios` | `estado.*` | omitir las vacías |
| `comparte_foto_con` | `imagen` | `"img/<ese-codigo>.jpg"`; omitir si viene vacía |
| `detalle_*` | `detalle` | ver abajo |

Las 16 columnas `detalle_*` se convierten en el objeto `detalle`, **salteando las
vacías** y con la clave capitalizada como la espera el sitio:

| Columna | Clave en el JSON | Categorías que la llenan |
| --- | --- | --- |
| `detalle_pantalla` | `Pantalla` | iPhone, Mac, iPad, Relojes |
| `detalle_procesador` | `Procesador` | iPhone, Mac, iPad |
| `detalle_memoria` | `Memoria` | Mac |
| `detalle_almacenamiento` | `Almacenamiento` | iPhone, Mac, iPad |
| `detalle_camara` | `Cámara` | iPhone, iPad |
| `detalle_bateria` | `Batería` | todas |
| `detalle_conectividad` | `Conectividad` | iPad, Accesorios |
| `detalle_compatibilidad` | `Compatibilidad` | iPad, Accesorios |
| `detalle_puertos` | `Puertos` | Mac |
| `detalle_conector` | `Conector` | iPhone |
| `detalle_color` | `Color` | iPhone, Mac, iPad |
| `detalle_tipo` | `Tipo` | Accesorios |
| `detalle_resistencia` | `Resistencia` | Accesorios |
| `detalle_incluye` | `Incluye` | Accesorios |
| `detalle_origen` | `Origen` | Accesorios |
| `detalle_estado` | `Estado` | iPhone, Mac, Accesorios |

**Ojo con el orden:** las claves de `detalle` se muestran en el orden en que están
escritas en el JSON, así que al convertir conviene respetar el orden de la
categoría (ver "Juego fijo propuesto" más arriba), no el orden de las columnas.

**Codificación:** el CSV está en UTF-8. Si al abrirlo en Excel los acentos se ven
mal, hay que importarlo eligiendo UTF-8 en vez de hacer doble clic. En Google
Sheets se abre bien directo.

---

## Qué pedirle al cliente

Esto es lo concreto que tiene que mandar para que el catálogo quede cargado.

### 1. La planilla completa

El archivo **`plantilla-productos.csv`** de este mismo repo, lleno. Se abre con
Excel, Google Sheets o LibreOffice.

- Tiene una **fila de instrucciones** (la segunda) y una **fila de ejemplo** (la
  tercera). Las dos hay que **borrarlas** antes de mandarla.
- Una fila por producto. **Los usados van en su propia fila**, con su propio
  código, no como una columna del nuevo.
- Las columnas obligatorias son las cuatro marcadas: código, nombre, categoría y
  precio. Sin esas cuatro el producto no se puede cargar.
- Los precios van **sin puntos y sin el signo $**: `1749000`.
- Los combos van en la pestaña/planilla aparte o al final, indicando: los dos
  productos que lo forman y el porcentaje de descuento. **El precio del combo no
  se calcula a mano.**

### 2. Las fotos

- **Una carpeta** con todas las fotos juntas.
- **Formato `.jpg`**, cuadradas de **1000 × 1000 px**, producto centrado y fondo
  blanco.
- **El nombre de cada archivo tiene que ser el código del producto** de la
  planilla, tal cual, más `.jpg`. Si en la planilla el código es
  `iphone-15-128`, la foto se llama `iphone-15-128.jpg`. Es lo que conecta la
  foto con el producto: si el nombre no coincide, la foto no aparece.
- Si un producto tiene más de una foto, las adicionales llevan el mismo nombre
  con `-2`, `-3`, `-4`, `-5`. Máximo 5 en total.
- Si dos productos comparten foto (por ejemplo un modelo nuevo y su versión
  usada), **no hace falta mandarla dos veces**: alcanza con avisarlo.

### 3. Datos que NO entran en la planilla

Hay que pedirlos aparte, porque no son del catálogo pero sin ellos el sitio no se
publica:

- **Nombre del comercio**, tal cual quiere que se lea en el logotipo y en los
  mensajes de WhatsApp.
- **Número de WhatsApp**, con característica, el que va a recibir los pedidos.
- **Usuario de Instagram** (sin el `@`), o avisar si no tiene: el botón
  desaparece solo.
- **Dirección del local** y **localidad**, para el pie de página y el encabezado.
- **Zona de envíos real**: a qué localidades llega y si hay costo.
- **Si hay local a la calle** para retirar, y con qué horarios.
- **Formas de pago** que acepta y sus condiciones reales (cuotas, recargos,
  descuento por transferencia). Hoy los textos son provisorios y no prometen
  nada.
- **Promoción bancaria vigente**, si la tiene, y con qué tarjetas.
- **Plazo de entrega estimado** que se anima a sostener.
- **Qué garantía ofrece**, si ofrece alguna. Es la pregunta del FAQ que hoy no
  promete nada a propósito.
- **Logo o inicial** para el ícono de la pestaña del navegador.

### 4. Antes de publicar

Recordarle que el sitio tiene puesto un bloqueo para que Google no lo indexe
mientras está en pruebas, y que **hay que sacarlo antes de publicar de verdad**.
Está documentado en `PERSONALIZAR.md`.
