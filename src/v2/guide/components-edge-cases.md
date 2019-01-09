---
Título: Manejo de casos de Edge
tipo: guía
orden: 106
---

&gt; Esta página asume que ya ha leído los [Conceptos básicos de los componentes] (components.html). Lea eso primero si es nuevo en componentes.

<p class="tip"> Todas las funciones en esta página documentan el manejo de casos perimetrales, es decir, situaciones inusuales que a veces requieren doblar un poco las reglas de Vue. Sin embargo, tenga en cuenta que todos ellos tienen desventajas o situaciones en las que podrían ser peligrosos. Estos se anotan en cada caso, así que téngalos en cuenta cuando decida utilizar cada función. </p>

## Elemento y acceso a componentes

En la mayoría de los casos, es mejor evitar acceder a otras instancias de componentes o manipular manualmente los elementos DOM. Hay casos, sin embargo, cuando puede ser apropiado.

### Accediendo a la instancia raíz

En cada subcomponente de una instancia de `new Vue`, se puede acceder a esta instancia raíz con la propiedad` $ root`. Por ejemplo, en esta instancia raíz:

`` `js
// La instancia raíz de Vue
nueva vista ({
datos: {
foo: 1
}
calculado: {
barra: función () {/ * ... * /}
}
métodos: {
baz: function () {/ * ... * /}
}
})
`` `

Todos los subcomponentes ahora podrán acceder a esta instancia y usarla como una tienda global:

`` `js
// obtener datos de root
esto. $ root.foo

// Establecer datos de raíz
este. $ root.foo = 2

// Acceso a las propiedades calculadas de la raíz
este. $ root.bar

// Métodos de raíz de llamada
este. $ root.baz ()
`` `

<p class="tip"> Esto puede ser conveniente para demostraciones o aplicaciones muy pequeñas con un puñado de componentes. Sin embargo, el patrón no se adapta bien a aplicaciones de mediana o gran escala, por lo que recomendamos encarecidamente utilizar <a href="https://github.com/vuejs/vuex">Vuex</a> para administrar el estado en la mayoría de los casos. </p>

### Accediendo a la instancia del componente padre

Similar a `$ root`, la propiedad` $ parent` se puede usar para acceder a la instancia principal desde un elemento secundario. Esto puede ser tentador de alcanzar como una alternativa perezosa a pasar datos con una prop.

<p class="tip"> En la mayoría de los casos, recurrir al elemento principal hace que su aplicación sea más difícil de depurar y comprender, especialmente si se muta la información en el elemento principal. Cuando analice ese componente más adelante, será muy difícil averiguar de dónde provino esa mutación. </p>

Sin embargo, hay casos, particularmente bibliotecas de componentes compartidas, cuando este _puede_ ser apropiado. Por ejemplo, en los componentes abstractos que interactúan con las API de JavaScript en lugar de representar HTML, como estos componentes hipotéticos de Google Maps:

`` `html
<google-map>
<google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
</google-map>
`` `

El ` <google-map> El componente `puede definir una propiedad` map` a la que todos los subcomponentes necesitan acceso. En este caso <google-map-markers> `podría querer acceder a ese mapa con algo como` this. $ parent.getMap`, para agregarle un conjunto de marcadores. Puede ver este patrón [en acción aquí] (https://jsfiddle.net/chrisvfritz/ttzutdxh/).

Sin embargo, tenga en cuenta que los componentes construidos con este patrón aún son intrínsecamente frágiles. Por ejemplo, imagina que agregamos un nuevo ` <google-map-region> `componente y cuando` <google-map-markers> `aparece dentro de eso, solo debe mostrar marcadores que caigan dentro de esa región:

`` `html
<google-map>
<google-map-region v-bind:shape="cityBoundaries">
<google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
</google-map-region>
</google-map>
`` `

Entonces dentro <google-map-markers> `Puede que te encuentres buscando un truco como este

`` `js
var map = this. $ parent.map || esto. $ parent. $ parent.map
`` `

Esto se ha ido rápidamente de las manos. Es por eso que para proporcionar información de contexto a componentes descendentes de profundidad arbitraria, en su lugar recomendamos [inyección de dependencia] (# Dependencia-Inyección).

### Acceso a instancias de componentes secundarios y elementos secundarios

A pesar de la existencia de accesorios y eventos, a veces es posible que aún necesite acceder directamente a un componente secundario en JavaScript. Para lograr esto, puede asignar una ID de referencia al componente secundario utilizando el atributo `ref`. Por ejemplo:

`` `html
<base-input ref="usernameInput"></base-input>
`` `

Ahora en el componente donde ha definido este `ref`, puede usar:

`` `js
este. $ refs.usernameInput
`` `

para acceder al ` <base-input> `instancia. Esto puede ser útil cuando, por ejemplo, desea enfocar esta entrada de un padre mediante programación. En ese caso, el ` <base-input> El componente `puede usar un` ref` de manera similar para proporcionar acceso a elementos específicos dentro de él, como:

`` `html
<input ref="input">
`` `

E incluso definir métodos para su uso por el padre:

`` `js
métodos: {
// Se usa para enfocar la entrada del padre
focus: function () {
este. $ refs.input.focus ()
}
}
`` `

De este modo, se permite al componente principal enfocar la entrada dentro de ` <base-input> `con:

`` `js
este. $ refs.usernameInput.focus ()
`` `

Cuando `ref` se usa junto con` v-for`, la referencia que obtendrás será una matriz que contiene los componentes secundarios que reflejan la fuente de datos.

<p class="tip"> <code>$refs</code> solo se completan después de que el componente se haya procesado y no sean reactivos. Solo está pensado como una trampilla de escape para la manipulación directa de niños: debe evitar el acceso a <code>$refs</code> desde plantillas o propiedades computadas. </p>

### Inyección de dependencia

Anteriormente, cuando describimos [Acceso a la instancia del componente principal] (# Acceso a la instancia del componente principal), mostramos un ejemplo como este:

`` `html
<google-map>
<google-map-region v-bind:shape="cityBoundaries">
<google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
</google-map-region>
</google-map>
`` `

En este componente, todos los descendientes de <google-map> `necesitaba acceso a un método` getMap`, para saber con qué mapa interactuar. Desafortunadamente, el uso de la propiedad `$ parent` no se ajustó bien a componentes más profundamente anidados. Ahí es donde la inyección de dependencia puede ser útil, utilizando dos nuevas opciones de instancia: `proporcionar` e` inyectar &#39;.

Las opciones &#39;proporcionar&#39; nos permiten especificar los datos / métodos que queremos ** proporcionar ** a los componentes descendientes. En este caso, ese es el método `getMap` dentro de` <google-map> `:

`` `js
proporcionar: function () {
regreso {
getMap: this.getMap
}
}
`` `

Luego, en cualquier descendiente, podemos usar la opción &#39;inyectar&#39; para recibir propiedades específicas que nos gustaría agregar a esa instancia:

`` `js
inyectar: [&#39;getMap&#39;]
`` `

Puede ver el [ejemplo completo aquí] (https://jsfiddle.net/chrisvfritz/tdv8dt3s/). La ventaja sobre el uso de `$ parent` es que podemos acceder a` getMap` en cualquier componente descendiente, sin exponer la instancia completa de ` <google-map> `. Esto nos permite seguir desarrollando ese componente de manera más segura, sin temor a que podamos cambiar / eliminar algo en lo que un componente hijo confía. La interfaz entre estos componentes permanece claramente definida, al igual que con `props`.

De hecho, puede pensar en la inyección de dependencia como una especie de &quot;accesorios de largo alcance&quot;, excepto:

* los componentes de los antepasados no necesitan saber qué descendientes usan las propiedades que proporciona
* los componentes descendientes no necesitan saber de dónde vienen las propiedades inyectadas

<p class="tip"> Sin embargo, hay desventajas a la inyección de dependencia. Combina los componentes de su aplicación con la forma en que están organizados actualmente, lo que dificulta la refactorización. Las propiedades proporcionadas tampoco son reactivas. Esto es por diseño, porque usarlos para crear un almacén de datos central se escala tan mal como <a href="#Accessing-the-Root-Instance">usar <code>$root</code></a> para el mismo propósito. Si las propiedades que desea compartir son específicas de su aplicación, en lugar de genéricas, o si alguna vez desea actualizar los datos proporcionados dentro de los antepasados, es una buena señal de que probablemente necesite una solución de administración de bienes <a href="https://github.com/vuejs/vuex">inmuebles</a> como <a href="https://github.com/vuejs/vuex">Vuex</a> . </p>

Obtenga más información sobre la inyección de dependencia en [el documento API] (https://vuejs.org/v2/api/#provide-inject).

## Oyentes de eventos programáticos

Hasta ahora, ha visto usos de `$ emit`, escuchado con` v-on`, pero las instancias de Vue también ofrecen otros métodos en su interfaz de eventos. Podemos:

- Escuche un evento con `$ on (eventName, eventHandler)`
- Escuche un evento solo una vez con `$ once (eventName, eventHandler)`
- Deja de escuchar un evento con `$ off (eventName, eventHandler)`

Normalmente, no tendrá que usarlos, pero están disponibles para los casos en que necesita escuchar manualmente los eventos en una instancia de componente. También pueden ser útiles como herramienta de organización de código. Por ejemplo, a menudo puede ver este patrón para integrar una biblioteca de terceros:

`` `js
// Adjuntar el selector de fecha a una entrada una vez
// está montado en el DOM.
montado: function () {
// Pikaday es una biblioteca de seleccionadores de fecha de terceros
this.picker = new Pikaday ({
campo: este. $ refs.input,
formato: &#39;YYYY-MM-DD&#39;
})
}
// Justo antes de que el componente sea destruido,
// también destruye el selector de fechas.
beforeDestroy: function () {
este.picker.destroy ()
}
`` `

Esto tiene dos problemas potenciales:

- Requiere guardar el `picker` en la instancia del componente, cuando es posible que solo los ganchos del ciclo de vida tengan acceso a él. Esto no es terrible, pero podría considerarse desorden.
- Nuestro código de configuración se mantiene separado de nuestro código de limpieza, lo que dificulta la limpieza programática de todo lo que configuramos.

Podría resolver ambos problemas con un oyente programático:

`` `js
montado: function () {
selector var = pikaday nuevo ({
campo: este. $ refs.input,
formato: &#39;YYYY-MM-DD&#39;
})

este. $ once (&#39;hook: beforeDestroy&#39;, function () {
picker.destroy ()
})
}
`` `

Usando esta estrategia, incluso podríamos usar Pikaday con varios elementos de entrada, con cada nueva instancia limpiando automáticamente después de sí misma:

`` `js
montado: function () {
this.attachDatepicker (&#39;startDateInput&#39;)
this.attachDatepicker (&#39;endDateInput&#39;)
}
métodos: {
attachDatepicker: function (refName) {
selector var = pikaday nuevo ({
campo: este. $ refs [refName],
formato: &#39;YYYY-MM-DD&#39;
})

este. $ once (&#39;hook: beforeDestroy&#39;, function () {
picker.destroy ()
})
}
}
`` `

Vea [este violín] (https://jsfiddle.net/chrisvfritz/1Leb7up8/) para el código completo. Sin embargo, tenga en cuenta que si tiene que realizar muchas tareas de configuración y limpieza dentro de un solo componente, la mejor solución será crear más componentes modulares. En este caso, le recomendamos crear un `reutilizable <input-datepicker> `componente.

Para obtener más información sobre los oyentes programáticos, consulte la API para [Métodos de instancia de eventos] (https://vuejs.org/v2/api/#Instance-Methods-Events).

<p class="tip"> Tenga en cuenta que el sistema de eventos de Vue es diferente de la <a href="https://developer.mozilla.org/en-US/docs/Web/API/EventTarget">API EventTarget</a> del navegador. Aunque funcionan de manera similar, <code>$emit</code> , <code>$on</code> y <code>$off</code> <strong>no</strong> son alias para <code>dispatchEvent</code> , <code>addEventListener</code> y <code>removeEventListener</code> . </p>

## Referencias circulares

### Componentes recursivos

Los componentes pueden invocarse recursivamente en su propia plantilla. Sin embargo, solo pueden hacerlo con la opción `name`:

`` `js
nombre: &#39;nombre-único-de-mi-componente&#39;
`` `

Cuando registra un componente globalmente con `Vue.component`, el ID global se establece automáticamente como la opción` nombre` del componente.

`` `js
Vue.component (&#39;nombre único de mi componente&#39;, {
// ...
})
`` `

Si no tienes cuidado, los componentes recursivos también pueden llevar a bucles infinitos:

`` `js
nombre: &#39;desbordamiento de pila&#39;,
modelo: &#39; <div><stack-overflow></stack-overflow></div> &#39;
`` `

Un componente como el anterior generará un error de &quot;tamaño de pila máximo excedido&quot;, así que asegúrese de que la invocación recursiva sea condicional (es decir, use un `v-if` que eventualmente será` falso`).

### Referencias circulares entre componentes

Digamos que está creando un árbol de directorios de archivos, como en Finder o File Explorer. Es posible que tenga un componente `tree-folder` con esta plantilla:

`` `html
<p>
<span>{{ nombre de la carpeta }}</span>
<tree-folder-contents :children="folder.children"/>
</p>
`` `

Luego un componente `tree-folder-contents` con esta plantilla:

`` `html
<ul>
<li v-for="child in children">
<tree-folder v-if="child.children" :folder="child"/>
<span v-else>{{ nombre de niño }}</span>
</li>
</ul>
`` `

Cuando mires detenidamente, verás que estos componentes serán realmente el descendiente y el ancestro del otro en el árbol de renderización, ¡una paradoja! Al registrar componentes globalmente con `Vue.component`, esta paradoja se resuelve automáticamente. Si eres tú, puedes dejar de leer aquí.

Sin embargo, si necesita / importa componentes utilizando un __module system__, por ejemplo, a través de Webpack o Browserify, obtendrá un error:

`` `
Error al montar el componente: la plantilla o la función de representación no está definida.
`` `

Para explicar lo que está sucediendo, llamemos a nuestros componentes A y B. El sistema de módulos ve que necesita A, pero primero A necesita B, pero B necesita A, pero A necesita B, etc. Está atascado en un bucle, sin saber cómo resuelva completamente cualquiera de los componentes sin resolver primero el otro. Para solucionar esto, debemos darle al sistema de módulos un punto en el que pueda decir: &quot;A necesita B _eventualmente_, pero no hay necesidad de resolver B primero&quot;.

En nuestro caso, hagamos de ese punto el componente `tree-folder`. Sabemos que el elemento secundario que crea la paradoja es el componente `tree-folder-contents`, por lo que esperaremos hasta el gancho del ciclo de vida` beforeCreate` para registrarlo:

`` `js
beforeCreate: function () {
este. $ options.components.TreeFolderContents = require (&#39;./ tree-folder-contents.vue&#39;). default
}
`` `

O alternativamente, puede usar la importación &#39;asincrónica&#39; de Webpack cuando registra el componente localmente:

`` `js
componentes: {
TreeFolderContents: () =&gt; import (&#39;./ tree-folder-contents.vue&#39;)
}
`` `

¡Problema resuelto!

## Definiciones de plantillas alternativas

### Plantillas en línea

Cuando el atributo especial `inline-template` está presente en un componente secundario, el componente utilizará su contenido interno como su plantilla, en lugar de tratarlo como contenido distribuido. Esto permite una creación de plantillas más flexible.

`` `html
<my-component inline-template>
<div>
<p> Estos se compilan como plantilla propia del componente. </p>
<p> No es el contenido de transclusión de los padres. </p>
</div>
</my-component>
`` `

<p class="tip"> Sin embargo, <code>inline-template</code> hace que sea más difícil razonar sobre el alcance de sus plantillas. Como práctica recomendada, prefiera definir plantillas dentro del componente usando la opción de <code>template</code> o en un elemento <code>&lt;template&gt;</code> en un archivo <code>.vue</code> . </p>

### X-Templates

Otra forma de definir plantillas es dentro de un elemento de secuencia de comandos con el tipo `text / x-template`, y luego se hace referencia a la plantilla mediante una identificación. Por ejemplo:

`` `html
<script type="text/x-template" id="hello-world-template">
<p> Hola hola hola </p>
</script>
`` `

`` `js
Vue.component (&#39;hello-world&#39;, {
plantilla: &#39;# hello-world-template&#39;
})
`` `

<p class="tip"> Estos pueden ser útiles para demostraciones con plantillas grandes o en aplicaciones extremadamente pequeñas, pero deben evitarse, ya que separan las plantillas del resto de la definición del componente. </p>

## Controlando Actualizaciones

Gracias al sistema Reactivity de Vue, siempre sabe cuándo actualizar (si lo usa correctamente). Sin embargo, hay casos extremos en los que es posible que desee forzar una actualización, a pesar del hecho de que no se ha modificado ningún dato reactivo. Luego hay otros casos en los que es posible que desee evitar actualizaciones innecesarias.

### forzando una actualización

<p class="tip"> Si necesita forzar una actualización en Vue, en el 99,99% de los casos, ha cometido un error en alguna parte. </p>

Es posible que no haya tenido en cuenta las advertencias de detección de cambios [con arreglos] (https://vuejs.org/v2/guide/list.html#Caveats) u [objetos] (https://vuejs.org/v2/guide/list .html # Objeto-Cambio-Detección-Advertencias), o puede estar confiando en un estado que no es rastreado por el sistema de reactividad de Vue, por ejemplo, con `data`.

Sin embargo, si ha descartado lo anterior y se encuentra en esta situación extremadamente rara de tener que forzar manualmente una actualización, puede hacerlo con [`$ forceUpdate`] (../ api / # vm-forceUpdate).

### Componentes estáticos baratos con `v-once`

La representación de elementos HTML simples es muy rápida en Vue, pero a veces es posible que tenga un componente que contenga ** mucho ** contenido estático. En estos casos, puede asegurarse de que solo se evalúe una vez y luego se almacene en caché agregando la directiva `v-once` al elemento raíz, como esto:

`` `js
Vue.component (&#39;términos de servicio&#39;, {
plantilla: `
<div v-once>
<h1> Términos de servicio </h1>
... una gran cantidad de contenido estático ...
</div>
`
})
`` `

<p class="tip"> Una vez más, trate de no abusar de este patrón. Si bien es conveniente en aquellos casos raros en los que tiene que generar gran cantidad de contenido estático, simplemente no es necesario a menos que realmente note una representación lenta, además, podría causar mucha confusión más adelante. Por ejemplo, imagine a otro desarrollador que no está familiarizado con <code>v-once</code> o simplemente lo pierde en la plantilla. Pueden pasar horas tratando de averiguar por qué la plantilla no se actualiza correctamente. </p>

