---
Título: Migración desde Vue Router 0.7.x
tipo: guía
orden: 702
---

&gt; Sólo Vue Router 2 es compatible con Vue 2, por lo que si está actualizando Vue, también deberá actualizar Vue Router. Es por eso que hemos incluido detalles sobre la ruta de migración aquí en los documentos principales. Para obtener una guía completa sobre el uso del nuevo enrutador Vue, consulte la [Documentos del enrutador Vue] (https://router.vuejs.org/en/).

## Inicialización del router

### `router.start` <sup>reemplazado</sup>

Ya no hay una API especial para inicializar una aplicación con Vue Router. Eso significa en lugar de:

`` `js
router.start ({
modelo: &#39; <router-view></router-view> &#39;
}, &#39;#app&#39;)
`` `

Pasas una propiedad de enrutador a una instancia de Vue:

`` `js
nueva vista ({
  el: '#app',
enrutador: enrutador,
modelo: &#39; <router-view></router-view> &#39;
})
`` `

O, si estás usando la versión de Vue solo en tiempo de ejecución:

`` `js
nueva vista ({
  el: '#app',
enrutador: enrutador,
render: h =&gt; h (&#39;enrutador-vista&#39;)
})
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>router.start</code> . </p>
</div>
{% endraw%}

## Definiciones de ruta

### `router.map` <sup>reemplazado</sup>

Las rutas ahora se definen como una matriz en una [opción de `` ``] (https://router.vuejs.org/en/essentials/getting-started.html#javascript) en la instanciación del enrutador. Así que estas rutas por ejemplo:

`` `js
router.map ({
&#39;/ foo&#39;: {
componente: foo
}
&#39;/bar&#39;: {
componente: barra
}
})
`` `

Será en cambio definido con:

`` `js
fue enrutador = nuevo VueRouter ({
rutas: [
{ruta: &#39;/ foo&#39;, componente: Foo},
{ruta: &#39;/ bar&#39;, componente: Barra}
]
})
`` `

La sintaxis de la matriz permite una coincidencia de ruta más predecible, ya que no se garantiza que la iteración sobre un objeto use el mismo orden de claves en los navegadores.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar ejemplos de <code>router.map</code> está llamando </p>
</div>
{% endraw%}

### `router.on` <sup>eliminado</sup>

Si necesita generar rutas mediante programación al iniciar su aplicación, puede hacerlo empujando dinámicamente las definiciones a una matriz de rutas. Por ejemplo:

`` `js
// Rutas base normales
rutas de var = [
// ...
]

// Rutas generadas dinámicamente
marketingPages.forEach (función (página) {
rutas.push ({
ruta: &#39;/ marketing /&#39; + page.slug
componente: {
extiende: MarketingComponent
datos: function () {
volver {página: página}
}
}
})
})

enrutador var = enrutador nuevo ({
rutas: rutas
})
`` `

Si necesita agregar nuevas rutas después de que se haya creado una instancia del enrutador, puede reemplazar el emparejador del enrutador por uno nuevo que incluya la ruta que desea agregar:

`` `js
router.match = createMatcher (
[{
ruta: &#39;/ mi / nuevo / ruta&#39;,
componente: MyComponent
}]. concat (router.options.routes)
)
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>router.on</code> . </p>
</div>
{% endraw%}

### `router.beforeEach` <sup>cambiado</sup>

`router.beforeEach` ahora funciona de forma asíncrona y toma una función` next` como tercer argumento.

`` `js
router.beforeEach (función (transición) {
if (transition.to.path === &#39;/ forbidden&#39;) {
transición.abort ()
} else {
transición.siguiente ()
}
})
`` `

`` `js
router.beforeEach (función (a, desde, siguiente) {
if (to.path === &#39;/ forbidden&#39;) {
siguiente (falso)
} else {
siguiente()
}
})
`` `

### `subRoutes` <sup>renombrado</sup>

[Renombrado a `children`] (https://router.vuejs.org/en/essentials/nested-routes.html) para mantener la coherencia dentro de Vue y con otras bibliotecas de enrutamiento.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción de <code>subRoutes</code> . </p>
</div>
{% endraw%}

### `router.redirect` <sup>reemplazado</sup>

Esta es ahora una [opción sobre definiciones de ruta] (https://router.vuejs.org/en/essentials/redirect-and-alias.html). Así por ejemplo, actualizarás:

`` `js
router.redirect ({
&#39;/ tos&#39;: &#39;/ términos de servicio&#39;
})
`` `

a una definición como la que se muestra a continuación en su configuración de `rutas`:

`` `js
{
ruta: &#39;/ tos&#39;,
redirigir: &#39;/ términos de servicio&#39;
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>router.redirect</code> . </p>
</div>
{% endraw%}

### `router.alias` <sup>reemplazado</sup>

Esta es ahora una [opción en la definición de la ruta] (https://router.vuejs.org/en/essentials/redirect-and-alias.html) al que desea hacer un alias. Así por ejemplo, actualizarás:

`` `js
router.alias ({
&#39;/ manage&#39;: &#39;/ admin&#39;
})
`` `

a una definición como la que se muestra a continuación en su configuración de `rutas`:

`` `js
{
ruta: &#39;/ admin&#39;,
componente: AdminPanel,
alias: &#39;/ manage&#39;
}
`` `

Si necesita varios alias, también puede usar una sintaxis de matriz:

`` `js
alias: [&#39;/ manage&#39;, &#39;/ manage&#39;, &#39;/ administrate&#39;]
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>router.alias</code> están llamando. </p>
</div>
{% endraw%}

### Propiedades de ruta arbitraria <sup>reemplazadas</sup>

Las propiedades de ruta arbitrarias ahora deben tener un alcance bajo la nueva meta propiedad, para evitar conflictos con características futuras. Así por ejemplo, si hubieras definido:

`` `js
&#39;/ admin&#39;: {
componente: AdminPanel,
requireAuth: true
}
`` `

Entonces ahora lo actualizarías a:

`` `js
{
ruta: &#39;/ admin&#39;,
componente: AdminPanel,
meta: {
requireAuth: true
}
}
`` `

Luego, cuando más tarde acceda a esta propiedad en una ruta, todavía pasará por meta. Por ejemplo:

`` `js
if (route.meta.requiresAuth) {
// ...
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de propiedades de ruta arbitrarias que no estén incluidas en el metadatos </p>
</div>
{% endraw%}

### [] Sintaxis para matrices en consultas <sup>eliminadas</sup>

Al pasar matrices para consultar parámetros, la sintaxis QueryString ya no es `/ foo? Users [] = Tom &amp; users [] = Jerry`, en cambio, la nueva sintaxis es` / foo? Users = Tom &amp; users = Jerry`. Internamente, `$ route.query.users` seguirá siendo un Array, pero si solo hay un parámetro en la consulta:` / foo? Users = Tom`, al acceder directamente a esta ruta, no hay forma de que el enrutador sepa si esperábamos que los &#39;usuarios&#39; fueran una matriz. Debido a esto, considere agregar una propiedad computada y reemplazar todas las referencias de `$ route.query.users` con ella:

`` `javascript
exportación predeterminada {
// ...
calculado: {
// los usuarios siempre serán una matriz
usuarios () {
const users = this. $ route.query.users
¿Volver Array.isArray (usuarios)? usuarios: [usuarios]
}
}
}
`` `

## coincidencia de ruta

La comparación de rutas ahora usa [path-to-regexp] (https://github.com/pillarjs/path-to-regexp) debajo del capó, lo que lo hace mucho más flexible que antes.

### Uno o más parámetros con nombre <sup>cambiados</sup>

La sintaxis ha cambiado ligeramente, por lo que las etiquetas `/ category / *`, por ejemplo, deben actualizarse a` / category /: tags + `.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la sintaxis de ruta obsoleta. </p>
</div>
{% endraw%}

## Enlaces

### `v-link` <sup>reemplazado</sup>

La directiva `v-link` ha sido reemplazada por una nueva [` <router-link> `componente] (https://router.vuejs.org/en/api/router-link.html), ya que este tipo de trabajo ahora es responsabilidad exclusiva de los componentes en Vue 2. Eso significa que siempre que tenga un enlace como este esta:

`` `html
<a v-link="'/about'">Acerca de</a>
`` `

Tendrás que actualizarlo así:

`` `html
<router-link to="/about"> Acerca de </router-link>
`` `

Tenga en cuenta que `target =&quot; _ blank &quot;` no es compatible con ` <router-link> `, así que si necesitas abrir un enlace en una nueva pestaña, debes usar` <a>`en su lugar.</a>

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la directiva <code>v-link</code> . </p>
</div>
{% endraw%}

### `v-link-active` <sup>reemplazado</sup>

La directiva `v-link-active` también ha sido reemplazada por el atributo` tag` en [el ` <router-link> `componente] (https://router.vuejs.org/en/api/router-link.html). Así por ejemplo, actualizarás esto:

`` `html
<li v-link-active>
<a v-link="'/about'">Acerca de</a>
</li>
`` `

a esto:

`` `html
<router-link tag="li" to="/about">
<a>Acerca de</a>
</router-link>
`` `

El ` <a>` será el enlace real (y obtendrá el href correcto), pero la clase activa se aplicará al `externo</a> <li> <a>`.</a>

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la directiva <code>v-link-active</code> . </p>
</div>
{% endraw%}

## Navegación programática

### `router.go` <sup>cambiado</sup>

Para mantener la coherencia con la [API de historial de HTML5] (https://developer.mozilla.org/en-US/docs/Web/API/History_API), `router.go` ahora solo se usa para [navegación hacia atrás / adelante] ( https://router.vuejs.org/en/essentials/navigation.html#routergon), mientras que [`router.push`] (https://router.vuejs.org/en/essentials/navigation.html#routerpushlocation) Se utiliza para navegar a una página específica.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">ayudante de la migración</a> de su base de código para encontrar ejemplos de <code>router.go</code> siendo utilizados en <code>router.push</code> se debe utilizar en su lugar. </p>
</div>
{% endraw%}

## Opciones de enrutador: modos

### `hashbang: false` <sup>eliminado</sup>

Hashbangs ya no es necesario para que Google rastree una URL, por lo que ya no es el valor predeterminado (o incluso una opción) para la estrategia hash.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>hashbang: false</code> . </p>
</div>
{% endraw%}

### `history: true` <sup>reemplazado</sup>

Todas las opciones de modo de enrutamiento se han condensado en una sola [`modalidad &#39;opción] (https://router.vuejs.org/en/api/options.html#mode). Actualizar:

`` `js
fue enrutador = nuevo VueRouter ({
historia: &#39;verdadero&#39;
})
`` `

a:

`` `js
fue enrutador = nuevo VueRouter ({
modo: &#39;historia&#39;
})
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>history: true</code> . </p>
</div>
{% endraw%}

### `abstract: true` <sup>reemplazado</sup>

Todas las opciones de modo de enrutamiento se han condensado en una sola [`modalidad &#39;opción] (https://router.vuejs.org/en/api/options.html#mode). Actualizar:

`` `js
fue enrutador = nuevo VueRouter ({
resumen: &#39;verdadero&#39;
})
`` `

a:

`` `js
fue enrutador = nuevo VueRouter ({
modo: &#39;abstracto&#39;
})
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>abstract: true</code> . </p>
</div>
{% endraw%}

## Opciones de ruta: Misc

### `saveScrollPosition` <sup>reemplazado</sup>

Esto se ha reemplazado con una [`scrollBehavior` option] (https://router.vuejs.org/en/advanced/scroll-behavior.html) que acepta una función, por lo que el comportamiento del desplazamiento es completamente personalizable, incluso por ruta . Esto abre muchas posibilidades nuevas, pero para replicar el antiguo comportamiento de:

`` `js
saveScrollPosition: true
`` `

Puedes reemplazarlo con:

`` `js
scrollBehavior: function (to, from, savedPosition) {
volver guardadoPosición || {x: 0, y: 0}
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>saveScrollPosition: true</code> . </p>
</div>
{% endraw%}

### `root` <sup>renombrado</sup>

Renombrado a `base` por coherencia con [el HTML` <base> `elemento] (https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>root</code> . </p>
</div>
{% endraw%}

### `transitionOnLoad` <sup>eliminado</sup>

Esta opción ya no es necesaria ahora que el sistema de transición de Vue tiene un control de transición explícito [`aparece`] (transitions.html # Transitions-on-Initial-Render)

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>transitionOnLoad: true</code> . </p>
</div>
{% endraw%}

### `suppressTransitionError` <sup>eliminado</sup>

Eliminado debido a la simplificación de los ganchos. Si realmente debe suprimir los errores de transición, puede usar [`try` ...` catch`] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try .. .catch) en su lugar.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>suppressTransitionError: true</code> . </p>
</div>
{% endraw%}

## Ganchos de ruta

### `activar` <sup>reemplazado</sup>

Utilice [`beforeRouteEnter`] (https://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) en el componente.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del <code>beforeRouteEnter</code> . </p>
</div>
{% endraw%}

### `canActivate` <sup>reemplazado</sup>

Utilice [`beforeEnter`] (https://router.vuejs.org/en/advanced/navigation-guards.html#perroute-guard) en la ruta.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del <code>canActivate</code> . </p>
</div>
{% endraw%}

### `desactivar` <sup>eliminado</sup>

Use los ganchos [`beforeDestroy`] del componente (../ api / # beforeDestroy) o [` destruidos`] (../ api / # destruidos) en su lugar.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del enlace de <code>deactivate</code> . </p>
</div>
{% endraw%}

### `canDeactivate` <sup>reemplazado</sup>

Utilice [`beforeRouteLeave`] (https://router.vuejs.org/en/advanced/navigation-guards.html#incomponent-guards) en el componente.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del <code>canDeactivate</code> . </p>
</div>
{% endraw%}

### `canReuse: false` <sup>eliminado</sup>

Ya no hay un caso de uso para esto en el nuevo enrutador Vue.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>canReuse: false</code> . </p>
</div>
{% endraw%}

### `datos` <sup>reemplazados</sup>

La propiedad `$ route` ahora es reactiva, por lo que puedes usar un observador para reaccionar a los cambios de ruta, como esto:

`` `js
reloj: {
&#39;$ ruta&#39;: &#39;fetchData&#39;
}
métodos: {
fetchData: function () {
// ...
}
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del enlace de <code>data</code> . </p>
</div>
{% endraw%}

### `$ loadingRouteData` <sup>eliminado</sup>

Defina su propia propiedad (por ejemplo, `isLoading`), luego actualice el estado de carga en un observador en la ruta. Por ejemplo, si está obteniendo datos con [axios] (https://github.com/mzabriskie/axios):

`` `js
datos: function () {
regreso {
mensajes: [],
isLoading: falso,
fetchError: nulo
}
}
reloj: {
&#39;$ ruta&#39;: función () {
var self = esto
self.isLoading = true
self.fetchData (). then (function () {
self.isLoading = false
})
}
}
métodos: {
fetchData: function () {
var self = esto
devuelve axios.get (&#39;/ api / posts&#39;)
.then (función (respuesta) {
self.posts = response.data.posts
})
.catch (función (error) {
self.fetchError = error
})
}
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la propiedad meta <code>$loadingRouteData</code> . </p>
</div>
{% endraw%}

