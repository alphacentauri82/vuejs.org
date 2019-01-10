---
Título: Funciones de render y JSX
tipo: guía
orden: 303
---

## Fundamentos

Vue recomienda utilizar plantillas para construir su HTML en la gran mayoría de los casos. Sin embargo, hay situaciones en las que realmente necesitas el poder programático completo de JavaScript. Ahí es donde puede usar la ** función de procesamiento **, una alternativa más cercana al compilador para las plantillas.

Vamos a sumergirnos en un ejemplo simple donde una función `render` sería práctica. Digamos que quieres generar encabezados anclados:

`` `html
<h1>
<a name="hello-world" href="#hello-world">
¡Hola Mundo!
</a>
</h1>
`` `

Para el código HTML anterior, decide que desea esta interfaz de componente:

`` `html
<anchored-heading :level="1"> ¡Hola Mundo! </anchored-heading>
`` `

Cuando comienzas con un componente que solo genera un encabezado basado en la prop &#39;level`, rápidamente llegas a esto:

`` `html
<script type="text/x-template" id="anchored-heading-template">
<h1 v-if="level === 1">
<slot></slot>
</h1>
<h2 v-else-if="level === 2">
<slot></slot>
</h2>
<h3 v-else-if="level === 3">
<slot></slot>
</h3>
<h4 v-else-if="level === 4">
<slot></slot>
</h4>
<h5 v-else-if="level === 5">
<slot></slot>
</h5>
<h6 v-else-if="level === 6">
<slot></slot>
</h6>
</script>
`` `

`` `js
Vue.component (&#39;título anclado&#39;, {
plantilla: &#39;# anclado-título-plantilla&#39;,
accesorios: {
nivel: {
teclea un número,
requerido: verdadero
}
}
})
`` `

Esa plantilla no se siente muy bien. No solo es detallado, sino que estamos duplicando ` <slot></slot> `para cada nivel de encabezado y tendremos que hacer lo mismo cuando agreguemos el elemento delimitador.

Si bien las plantillas funcionan bien para la mayoría de los componentes, está claro que este no es uno de ellos. Así que intentemos reescribirlo con una función `render`:

`` `js
Vue.component (&#39;título anclado&#39;, {
render: function (createElement) {
devuelve createElement (
&#39;h&#39; + this.level, // nombre de etiqueta
esto. $ slots.default // array of children
)
}
accesorios: {
nivel: {
teclea un número,
requerido: verdadero
}
}
})
`` `

Mucho más simple! Una especie de El código es más corto, pero también requiere una mayor familiaridad con las propiedades de la instancia de Vue. En este caso, debe saber que cuando pasa niños sin un atributo `slot` a un componente, como el` ¡Hola mundo! &#39;Dentro de `anchored-head`, esos niños se almacenan en la instancia del componente en` $ slots .default`. Si aún no lo ha hecho, ** se recomienda leer la [API de propiedades de la instancia] (../ api / # Instance-Properties) antes de sumergirse en las funciones de representación. **

## Nodos, árboles y el DOM virtual

Antes de sumergirnos en las funciones de render, es importante saber un poco sobre cómo funcionan los navegadores. Tomemos este HTML por ejemplo:

`` `html
<div>
<h1> Mi título </h1>
Algo de contenido de texto
<!-- TODO: Add tagline  -->
</div>
`` `

Cuando un navegador lee este código, crea un [árbol de &quot;nodos DOM&quot;] (https://javascript.info/dom-nodes) para ayudarlo a realizar un seguimiento de todo, tal como podría crear un árbol genealógico para realizar un seguimiento de su familia extendida.

El árbol de nodos DOM para el HTML anterior se ve así:

! [Visualización del árbol DOM] (/ images / dom-tree.png)

Cada elemento es un nodo. Cada pieza de texto es un nodo. Incluso los comentarios son nodos! Un nodo es solo una parte de la página. Y como en un árbol familiar, cada nodo puede tener hijos (es decir, cada pieza puede contener otras piezas).

Actualizar todos estos nodos de manera eficiente puede ser difícil, pero afortunadamente, nunca tiene que hacerlo manualmente. En cambio, le dices a Vue qué HTML quieres en la página, en una plantilla:

`` `html
<h1> {{blogTitle}} </h1>
`` `

O una función de render:

`` `js
render: function (createElement) {
devuelve createElement (&#39;h1&#39;, this.blogTitle)
}
`` `

Y en ambos casos, Vue mantiene la página actualizada automáticamente, incluso cuando cambia `blogTitle`.

### El DOM virtual

Vue logra esto al crear un ** DOM virtual ** para realizar un seguimiento de los cambios que debe realizar en el DOM real. Mirando de cerca esta línea:

`` `js
devuelve createElement (&#39;h1&#39;, this.blogTitle)
`` `

¿Qué está devolviendo `createElement`? No es _exactamente_ un elemento DOM real. Tal vez podría llamarse con más precisión `createNodeDescription`, ya que contiene información que describe a Vue qué tipo de nodo debe representar en la página, incluidas las descripciones de los nodos secundarios. Llamamos a esta descripción de nodo un &quot;nodo virtual&quot;, generalmente abreviado a ** VNode **. &quot;DOM virtual&quot; es lo que llamamos el árbol completo de VNodes, construido por un árbol de componentes de Vue.

## `createElement` Arguments

Lo siguiente con lo que tendrá que familiarizarse es cómo usar las características de la plantilla en la función `createElement`. Aquí están los argumentos que `createElement` acepta:

`` `js
// @returns {VNode}
crearElemento
// {String | Objeto | Función}
// Un nombre de etiqueta HTML, opciones de componente o async
// función resolviendo a uno de estos. Necesario.
&#39;div&#39;,

// {Objeto}
// Un objeto de datos correspondiente a los atributos.
// lo usarías en una plantilla. Opcional.
{
// (ver detalles en la siguiente sección a continuación)
}

// {String | Formación}
// VNodos de niños, creados usando `createElement ()`,
// o usando cadenas para obtener &#39;VNodos de texto&#39;. Opcional.
El
&#39;Un texto viene primero.&#39;,
createElement (&#39;h1&#39;, &#39;A headline&#39;),
createElement (MyComponent, {
accesorios: {
someProp: &#39;foobar&#39;
}
})
]
)
`` `

### El objeto de datos en profundidad

Una cosa a tener en cuenta: similar a cómo `v-bind: class` y` v-bind: style` tienen un tratamiento especial en las plantillas, tienen sus propios campos de nivel superior en los objetos de datos VNode. Este objeto también le permite enlazar atributos HTML normales así como propiedades DOM como `innerHTML` (esto reemplazaría la directiva` v-html`):

`` `js
{
// La misma API que `v-bind: class`, aceptando cualquiera
// una cadena, objeto, o matriz de cadenas y objetos.
clase: {
foo: cierto
bar: falso
}
// La misma API que `v-bind: style`, aceptando cualquiera
// una cadena, objeto, o matriz de objetos.
estilo: {
color rojo&#39;,
tamaño de fuente: &#39;14px&#39;
}
// Atributos normales de HTML
attrs: {
id: &#39;foo&#39;
}
// accesorios de componentes
accesorios: {
myProp: &#39;bar&#39;
}
// propiedades DOM
domProps: {
innerHTML: &#39;baz&#39;
}
// Los manejadores de eventos están anidados bajo `on`, aunque
// los modificadores como en `v-on: keyup.enter` no son
// soportado. Tendrás que revisar manualmente el
// keyCode en el controlador en su lugar.
en: {
haga clic en: this.clickHandler
}
// Sólo para componentes. Te permite escuchar
// eventos nativos, en lugar de eventos emitidos desde
// el componente usando `vm. $ emit`.
nativeOn: {
haga clic en: this.nativeClickHandler
}
// Directivas personalizadas. Tenga en cuenta que la &#39;vinculación&#39;
// `oldValue` no se puede configurar, ya que Vue realiza un seguimiento
// de eso para ti.
directivas: [
{
nombre: &#39;mi-directiva-personalizada&#39;,
valor: &#39;2&#39;,
expresión: &#39;1 + 1&#39;,
arg: &#39;foo&#39;,
modificadores: {
bar: cierto
}
}
]
// Slots de ámbito en forma de
// {nombre: props =&gt; VNodo | Formación <VNode> }
ranuras de ámbito: {
por defecto: props =&gt; createElement (&#39;span&#39;, props.text)
}
// El nombre de la ranura, si este componente es el
// hijo de otro componente
ranura: &#39;nombre de ranura&#39;,
// Otras propiedades especiales de nivel superior
tecla: &#39;myKey&#39;,
ref: &#39;myRef&#39;
}
`` `

### Ejemplo completo

Con este conocimiento, ahora podemos terminar el componente que comenzamos:

`` `js
var getChildrenTextContent = function (children) {
return children.map (función (nodo) {
return node.children
? getChildrenTextContent (node.children)
: node.text
}).unirse(&#39;&#39;)
}

Vue.component (&#39;título anclado&#39;, {
render: function (createElement) {
// crear id de kebab-case
var headingId = getChildrenTextContent (este. $ slots.default)
.toLowerCase ()
.replace (/ \ W + / g, &#39;-&#39;)
.replace (/ (^ \ - | \ - $) / g, &#39;&#39;)

devuelve createElement (
&#39;h&#39; + este.nivel,
El
createElement (&#39;a&#39;, {
attrs: {
nombre: headingId,
href: &#39;#&#39; + headerId
}
}, este. $ slots.default)
]
)
}
accesorios: {
nivel: {
teclea un número,
requerido: verdadero
}
}
})
`` `

### Restricciones

#### Los VNodos deben ser únicos

Todos los VNodes en el árbol de componentes deben ser únicos. Eso significa que la siguiente función de render no es válida:

`` `js
render: function (createElement) {
var myParagraphVNode = createElement (&#39;p&#39;, &#39;hi&#39;)
devuelve createElement (&#39;div&#39;, [
// ¡Ay, duplicar vnodos!
myParagraphVNode, myParagraphVNode
])
}
`` `

Si realmente desea duplicar el mismo elemento / componente muchas veces, puede hacerlo con una función de fábrica. Por ejemplo, la siguiente función de representación es una forma perfectamente válida de representar 20 párrafos idénticos:

`` `js
render: function (createElement) {
devuelve createElement (&#39;div&#39;,
Array.apply (null, {length: 20}). Map (function () {
devuelve createElement (&#39;p&#39;, &#39;hi&#39;)
})
)
}
`` `

## Reemplazo de las características de la plantilla con JavaScript plano

### `v-if` y` v-for`

Donde sea que se pueda lograr algo fácilmente en JavaScript simple, las funciones de representación de Vue no proporcionan una alternativa propietaria. Por ejemplo, en una plantilla usando `v-if` y` v-for`:

`` `html
<ul v-if="items.length">
<li v-for="item in items"> {{ nombre del árticulo }} </li>
</ul>
<p v-else> No se encontraron artículos. </p>
`` `

Esto podría ser reescrito con el JavaScript `if` /` else` y el `map` en una función de render:

`` `js
accesorios: [&#39;artículos&#39;],
render: function (createElement) {
if (this.items.length) {
devuelve createElement (&#39;ul&#39;, this.items.map (function (item) {
devuelve createElement (&#39;li&#39;, item.name)
}))
} else {
return createElement (&#39;p&#39;, &#39;No se encontraron elementos.&#39;)
}
}
`` `

### `v-model`

No hay una contraparte directa de `v-model` en las funciones de renderización, tendrá que implementar la lógica usted mismo:

`` `js
accesorios: [&#39;valor&#39;],
render: function (createElement) {
var self = esto
devuelve createElement (&#39;input&#39;, {
domProps: {
valor: self.value
}
en: {
entrada: función (evento) {
self. $ emit (&#39;input&#39;, event.target.value)
}
}
})
}
`` `

Este es el costo de ir a un nivel inferior, pero también le da mucho más control sobre los detalles de interacción en comparación con el `v-model`.

### Modificadores de eventos y claves

Para los modificadores de eventos `.passive`,` .capture` y `.once`, Vue ofrece prefijos que se pueden usar con` on`:

| Modificador (es) | Prefijo |
| ------ | ------ |
| `.passive` | `&amp;` |
| `.capture` | `!` |
| `.once` | `~` |
| `.capture.once` o <br> `.once.capture` | `~!` |

Por ejemplo:

`` `javascript
en: {
&#39;! click&#39;: this.doThisInCapturingMode,
&#39;~ keyup&#39;: this.doThisOnce,
&#39;~! mouseover&#39;: this.doThisOnceInCapturingMode
}
`` `

Para todos los demás modificadores de evento y clave, no es necesario ningún prefijo propietario, porque puede usar métodos de evento en el controlador:

| Modificador (es) | Equivalente en Handler |
| ------ | ------ |
| `.stop` | `event.stopPropagation ()` |
| `.prevent` | `event.preventDefault ()` |
| `.self` | `if (event.target! == event.currentTarget) return` |
| Llaves: <br> `.enter`,` .13` | `if (event.keyCode! == 13) return` (cambie` 13` a [otro código clave] (http://keycode.info/) para otros modificadores clave) |
| Modificadores de teclas: <br> `.ctrl`,` .alt`, `.shift`,` .meta` | `if (! event.ctrlKey) return` (cambia` ctrlKey` por `altKey`,` shiftKey`, o `metaKey`, respectivamente) |

Aquí hay un ejemplo con todos estos modificadores usados juntos:

`` `javascript
en: {
keyup: función (evento) {
// Cancelar si el elemento que emite el evento no es
// el elemento al que está vinculado el evento
if (event.target! == event.currentTarget) return
// Abortar si la clave que subió no es la entrada.
// tecla (13) y la tecla de cambio no se mantuvo presionada
// al mismo tiempo
if (! event.shiftKey || event.keyCode! == 13) return
// Detener la propagación del evento
event.stopPropagation ()
// Impedir el manejador de teclas predeterminado para este elemento
event.preventDefault ()
// ...
}
}
`` `

### ranuras

Puede acceder a los contenidos de las ranuras estáticas como Arrays de VNodes desde [`this. $ Slots`] (../ api / # vm-slots):

`` `js
render: function (createElement) {
// ` <div><slot></slot></div> `
devuelve createElement (&#39;div&#39;, this. $ slots.default)
}
`` `

Y acceda a las ranuras de ámbito como funciones que devuelven VNodos de [`this. $ ScopedSlots`] (../ api / # vm-scopedSlots):

`` `js
accesorios: [&#39;mensaje&#39;],
render: function (createElement) {
// ` <div><slot :text="message"></slot></div> `
devuelve createElement (&#39;div&#39;, [
este. $ scopedSlots.default ({
texto: este mensaje
})
])
}
`` `

Para pasar ranuras de ámbito a un componente secundario usando funciones de representación, use el campo `scopedSlots` en los datos de VNode:

`` `js
render: function (createElement) {
devuelve createElement (&#39;div&#39;, [
createElement (&#39;child&#39;, {
// pasa `scopedSlots` en el objeto de datos
// en la forma de {nombre: props =&gt; VNodo | Formación <VNode> }
ranuras de ámbito: {
por defecto: función (props) {
devuelve createElement (&#39;span&#39;, props.text)
}
}
})
])
}
`` `

## JSX

Si estás escribiendo muchas funciones `render`, puede que te resulte doloroso escribir algo como esto:

`` `js
crearElemento
&#39;título anclado&#39;, {
accesorios: {
nivel 1
}
}, [
createElement (&#39;span&#39;, &#39;Hello&#39;),
&#39;mundo!&#39;
]
)
`` `

Especialmente cuando la versión de la plantilla es tan simple en comparación:

`` `html
<anchored-heading :level="1">
<span>Hola</span> mundo
</anchored-heading>
`` `

Es por eso que hay un [complemento de Babel] (https://github.com/vuejs/babel-plugin-transform-vue-jsx) para usar JSX con Vue, lo que nos permite volver a una sintaxis más cercana a las plantillas:

`` `js
importar AnchoredHeading desde &#39;./AnchoredHeading.vue&#39;

nueva vista ({
  el: '#demo',
render: función (h) {
regreso (
<AnchoredHeading level={1}>
<span>Hola</span> mundo
</AnchoredHeading>
)
}
})
`` `

<p class="tip"> Aliasing `createElement` to` h` es una convención común que verá en el ecosistema Vue y en realidad es necesaria para JSX. Si `h` no está disponible en el alcance, su aplicación arrojará un error. </p>

Para obtener más información sobre cómo JSX se asigna a JavaScript, consulte [use docs] (https://github.com/vuejs/babel-plugin-transform-vue-jsx#usage).

## Componentes funcionales

El componente de encabezamiento anclado que creamos anteriormente es relativamente simple. No gestiona ningún estado, observa cómo se le pasa ningún estado y no tiene métodos de ciclo de vida. En realidad, es sólo una función con algunos accesorios.

En casos como este, podemos marcar los componentes como &quot;funcionales&quot;, lo que significa que no tienen estado (no [datos reactivos] (../ api / # Opciones-Datos)) y sin instancia (no &#39;este contexto&#39;). Un ** componente funcional ** se ve así:

`` `js
Vue.component (&#39;mi-componente&#39;, {
funcional: verdadero,
// Los apoyos son opcionales
accesorios: {
// ...
}
// Para compensar la falta de una instancia,
// Ahora se nos proporciona un segundo argumento de contexto.
render: function (createElement, context) {
// ...
}
})
`` `

&gt; Nota: en las versiones anteriores a 2.3.0, se requiere la opción `props` si desea aceptar props en un componente funcional. En 2.3.0+ puede omitir la opción `props` y todos los atributos encontrados en el nodo componente se extraerán implícitamente como props.

En 2.5.0+, si está utilizando [componentes de un solo archivo] (single-file-components.html), los componentes funcionales basados en plantillas se pueden declarar con:

`` `html
<template functional>
</template>
`` `

Todo lo que necesita el componente se pasa a través de `contexto`, que es un objeto que contiene:

- &#39;props`: un objeto de los props provistos.
- `children`: un conjunto de los niños VNode
- `slots`: una función que devuelve un objeto de slots
- `data`: todo el [objeto de datos] (# The-Data-Object-In-Depth), pasado al componente como el segundo argumento de` createElement`
- `parent`: una referencia al componente padre
- `listeners`: (2.3.0+) Un objeto que contiene detectores de eventos registrados por los padres. Este es un alias de `data.on`
- `inyecciones`: (2.3.0+) si usa la opción [` inyectar &#39;] (../ api / # proporcionar-inyectar), esto contendrá inyecciones resueltas.

Después de agregar `function: true`, actualizar la función de renderizado de nuestro componente de encabezado anclado requeriría agregar el argumento` context`, actualizar `this. $ Slots.default` a` context.children`, luego actualizar `this.level` a `context.props.level`.

Dado que los componentes funcionales son solo funciones, son mucho más baratos de representar. Sin embargo, la falta de una instancia persistente significa que no se mostrarán en el árbol de componentes [Vue devtools] (https://github.com/vuejs/vue-devtools).

También son muy útiles como componentes de envoltura. Por ejemplo, cuando necesitas:

- Elija programáticamente uno de varios otros componentes para delegar a
- Manipule niños, accesorios o datos antes de pasarlos a un componente secundario.

Aquí hay un ejemplo de un componente de &#39;lista inteligente&#39; que delega a componentes más específicos, dependiendo de los accesorios que se le pasen:

`` `js
era EmptyList = {/ * ... * /}
era TableList = {/ * ... * /}
fue OrderedList = {/ * ... * /}
var UnorderedList = {/ * ... * /}

Vue.component (&#39;lista inteligente&#39;, {
funcional: verdadero,
accesorios: {
artículos: {
Tipo: Array,
requerido: verdadero
}
isOrdered: Boolean
}
render: function (createElement, context) {
function properListComponent () {
var items = context.props.items

if (items.length === 0) devuelve EmptyList
if (typeof items [0] === &#39;object&#39;) devuelve TableList
if (context.props.isOrdered) devuelve OrderedList

volver UnorderedList
}

devuelve createElement (
properListComponent (),
context.data,
context.children
)
}
})
`` `

### Pasando atributos y eventos a elementos / componentes secundarios

En los componentes normales, los atributos no definidos como props se agregan automáticamente al elemento raíz del componente, reemplazando o [fusionando inteligentemente con] (class-and-style.html) cualquier atributo existente del mismo nombre.

Sin embargo, los componentes funcionales requieren que defina explícitamente este comportamiento:

`` `js
Vue.component (&#39;mi-botón-funcional&#39;, {
funcional: verdadero,
render: function (createElement, context) {
// Pasar de forma transparente todos los atributos, oyentes de eventos, niños, etc.
devuelve createElement (&#39;button&#39;, context.data, context.children)
}
})
`` `

Al pasar `context.data` como segundo argumento a` createElement`, estamos transmitiendo los atributos o escuchas de eventos utilizados en `my-function-button`. De hecho, es tan transparente que los eventos ni siquiera requieren el modificador `.native`.

Si está utilizando componentes funcionales basados en plantillas, también tendrá que agregar atributos y escuchas manualmente. Ya que tenemos acceso a los contenidos de contexto individuales, podemos usar `data.attrs` para pasar cualquier atributo HTML y` listeners` _ (el alias para `data.on`) _ para pasar a lo largo de cualquier evento.

`` `html
<template functional>
<button
class = &quot;btn btn-primary&quot;
v-bind = &quot;data.attrs&quot;
v-on = &quot;oyentes&quot;
&gt;
<slot/>
</button>
</template>
`` `

### `slots ()` vs `children`

Quizás te preguntes por qué necesitamos tanto `slots ()` como `children`. ¿No sería `slots (). Default` igual que` children`? En algunos casos, sí, pero ¿qué sucede si tiene un componente funcional con los siguientes hijos?

`` `html
<my-functional-component>
<p slot="foo">
primero
</p>
<p> segundo </p>
</my-functional-component>
`` `

Para este componente, `children` te dará los dos párrafos,` slots (). Default` te dará solo el segundo, y `slots (). Foo` te dará solo el primero. Por lo tanto, tener `children` y` slots () `le permite elegir si este componente conoce un sistema de tragamonedas o tal vez delegue esa responsabilidad a otro componente al transmitir“ children ”.

## Compilación de plantillas

Tal vez le interese saber que las plantillas de Vue realmente se compilan para representar funciones. Este es un detalle de implementación que normalmente no necesita conocer, pero si desea ver cómo se compilan las características específicas de la plantilla, puede encontrarlo interesante. A continuación se muestra una pequeña demostración que usa `Vue.compile` para compilar en vivo una cadena de plantilla:

{% raw%}
<div id="vue-compile-demo" class="demo">
<textarea v-model="templateText" rows="10"></textarea>
<div v-if="typeof result === 'object'">
<label>hacer:</label>
<pre><code>{{ result.render }}</code></pre>
<label>staticRenderFns:</label>
<pre v-for="(fn, index) in result.staticRenderFns"><code>_m({{ index }}): {{ fn }}</code></pre>
<pre v-if="!result.staticRenderFns.length"><code>{{ result.staticRenderFns }}</code></pre>
</div>
<div v-else>
<label>Error de compilación:</label>
<pre><code>{{ result }}</code></pre>
</div>
</div>
<script>
nueva vista ({
  el: '#vue-compile-demo',
datos: {
templateText: &#39;\
<div> \norte\
<header> \norte\
<h1> Soy una plantilla! </h1> \ n \
</header> \norte\
<p v-if="message"> \norte\
{{mensaje}} \ n \
</p> \norte\
<p v-else> \norte\
Ningún mensaje. \ N \
</p> \norte\
</div> \
&#39;,
}
calculado: {
resultado: function () {
if (! this.templateText) {
devuelve &#39;Introduce una plantilla válida arriba&#39;
}
tratar {
var resultado = Vue.compile (this.templateText.replace (/ \ s {2,} / g, &#39;&#39;))
regreso {
render: this.formatFunction (result.render),
staticRenderFns: result.staticRenderFns.map (this.formatFunction)
}
} captura (error) {
mensaje de error de retorno
}
}
}
métodos: {
formatFunction: function (fn) {
devuelve fn.toString (). replace (/ (\ {\ n) (\ S) /, &#39;$ 1 $ 2&#39;)
}
}
})
console.error = function (error) {
lanzar nuevo error (error)
}
</script>
<style>
#vue-compile-demo {
-webkit-user-select: heredar;
selección de usuario: heredar;
}
#vue-compile-demo pre {
relleno: 10px;
overflow-x: auto;
}
# ver-compilar-código de demostración {
espacio en blanco: pre;
relleno: 0
}
#vue-compile-demo textarea {
ancho: 100%;
Familia tipográfica: monoespacio;
}
</style>
{% endraw%}

