---
Título: Migración desde Vue 1.x
tipo: guía
orden: 701
---

## PREGUNTAS MÁS FRECUENTES

&gt; Woah - esta es una página super larga! ¿Eso significa que 2.0 es completamente diferente, tendré que aprender los conceptos básicos de nuevo y migrar será prácticamente imposible?

Me alegra que hayas preguntado! La respuesta es no. Aproximadamente el 90% de la API es el mismo y los conceptos básicos no han cambiado. Es largo porque nos gusta ofrecer explicaciones muy detalladas e incluir muchos ejemplos. Tenga la seguridad de que __ esto no es algo que tenga que leer de arriba a abajo __

&gt; ¿Dónde debo comenzar en una migración?

1. Comience ejecutando el [asistente de migración] (https://github.com/vuejs/vue-migration-helper) en un proyecto actual. Hemos minimizado y comprimido cuidadosamente un desarrollador Vue senior en una sencilla interfaz de línea de comandos. Cada vez que reconocen una característica obsoleta, le informarán, ofrecerán sugerencias y proporcionarán enlaces a más información.

2. Después de eso, navegue por la tabla de contenido de esta página en la barra lateral. Si ve un tema que le puede afectar, pero el asistente de migración no lo detectó, verifíquelo.

3. Si tiene alguna prueba, ejecútela y vea lo que todavía falla. Si no tiene pruebas, simplemente abra la aplicación en su navegador y preste atención a las advertencias o errores a medida que navega.

4. Por ahora, tu aplicación debería estar completamente migrada. Sin embargo, si aún tiene hambre de más, puede leer el resto de esta página o sumergirse en la guía nueva y mejorada desde [el principio] (index.html). Muchas partes serán esquemáticas, ya que ya está familiarizado con los conceptos básicos.

&gt; ¿Cuánto tiempo llevará migrar una aplicación Vue 1.x a 2.0?

Depende de algunos factores:

- El tamaño de su aplicación (las aplicaciones pequeñas a medianas probablemente serán menos de un día)

- ¿Cuántas veces te distraes y empiezas a jugar con una nueva característica genial? 😉 ¡No juzgando, también nos sucedió mientras construimos 2.0!

- Qué características obsoletas estás usando. La mayoría se puede actualizar con buscar y reemplazar, pero otros pueden tardar unos minutos. Si actualmente no estás siguiendo las mejores prácticas, Vue 2.0 también intentará forzarte a hacerlo. Esto es algo bueno a largo plazo, pero también podría significar un refactor significativo (aunque posiblemente vencido).

&gt; Si actualizo a Vue 2, ¿también tendré que actualizar Vuex y Vue Router?

Solo Vue Router 2 es compatible con Vue 2, así que sí, también tendrá que seguir la [ruta de migración para Vue Router] (migration-vue-router.html). Afortunadamente, la mayoría de las aplicaciones no tienen mucho código de enrutador, por lo que es probable que esto no tarde más de una hora.

En cuanto a Vuex, incluso la versión 0.8 es compatible con Vue 2, por lo que no está obligado a actualizar. Es posible que la única razón por la que desee actualizar de inmediato sea para aprovechar las nuevas funciones de Vuex 2, como los módulos y la placa de reducción reducida.

## Plantillas

### Fragmento de instancias <sup>eliminadas</sup>

Cada componente debe tener exactamente un elemento raíz. Las instancias de fragmentos ya no están permitidas. Si tienes una plantilla como esta:

`` `html
<p> foo </p>
<p> bar </p>
`` `

Se recomienda envolver todo el contenido en un nuevo elemento, como este:

`` `html
<div>
<p> foo </p>
<p> bar </p>
</div>
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su suite o aplicación de prueba de extremo a extremo después de la actualización y busque <strong>advertencias de la consola</strong> sobre varios elementos raíz en una plantilla. </p>
</div>
{% endraw%}

## Ganchos de ciclo de vida

### `beforeCompile` <sup>eliminado</sup>

Utilice el gancho `created` en su lugar.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar todos los ejemplos de este gancho. </p>
</div>
{% endraw%}

### `compiled` <sup>reemplazado</sup>

Utilice el nuevo gancho `mount` en su lugar.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar todos los ejemplos de este gancho. </p>
</div>
{% endraw%}

### `adjunto` <sup>eliminado</sup>

Utilice una comprobación personalizada en DOM en otros ganchos. Por ejemplo, para reemplazar:

`` `js
adjunto: función () {
hacer algo()
}
`` `

Podrías usar:

`` `js
montado: function () {
esto. $ nextTick (function () {
hacer algo()
})
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar todos los ejemplos de este gancho. </p>
</div>
{% endraw%}

### `desapegado` <sup>eliminado</sup>

Utilice una comprobación personalizada en DOM en otros ganchos. Por ejemplo, para reemplazar:

`` `js
desligado: function () {
hacer algo()
}
`` `

Podrías usar:

`` `js
destruido: función () {
esto. $ nextTick (function () {
hacer algo()
})
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar todos los ejemplos de este gancho. </p>
</div>
{% endraw%}

### `init` <sup>renombrado</sup>

Use el nuevo gancho `beforeCreate` en su lugar, que es esencialmente lo mismo. Se le cambió el nombre por coherencia con otros métodos del ciclo de vida.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar todos los ejemplos de este gancho. </p>
</div>
{% endraw%}

### `listo` <sup>reemplazado</sup>

Utilice el nuevo gancho `mount` en su lugar. Se debe tener en cuenta que con `mount &#39;, no hay garantía de estar en el documento. Para eso, también incluye `Vue.nextTick` /` vm. $ NextTick`. Por ejemplo:

`` `js
montado: function () {
esto. $ nextTick (function () {
// código que asume esto. $ el está en el documento
})
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar todos los ejemplos de este gancho. </p>
</div>
Array

## `v-for`

### `v-for` Orden de Argumento para Arreglos <sup>cambiado</sup>

Cuando se incluye un `índice`, el orden de los argumentos para las matrices solía ser` (índice, valor) `. Ahora es `(value, index)` ser más consistente con los métodos de matriz nativa de JavaScript, como `forEach` y` map`.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del orden de argumentos obsoletos. Tenga en cuenta que si nombra los argumentos de su índice como algo inusual como <code>position</code> o <code>num</code> , el ayudante no los marcará. </p>
</div>
{% endraw%}

### `v-for` Orden de argumento para objetos <sup>cambiado</sup>

Cuando se incluye una `clave`, el orden de los argumentos para los objetos solía ser` (clave, valor) `. Ahora es `(value, key)` ser más coherente con los iteradores de objetos comunes como el de lodash.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del orden de argumentos obsoletos. Tenga en cuenta que si asigna un nombre a sus argumentos clave como <code>name</code> o <code>property</code> , el ayudante no los marcará. </p>
</div>
{% endraw%}

### `$ index` y` $ key` <sup>eliminados</sup>

Las variables `$ index` y` $ clave &#39;asignadas implícitamente se han eliminado a favor de definirlas explícitamente en `v-for`. Esto hace que el código sea más fácil de leer para los desarrolladores con menos experiencia con Vue y también produce un comportamiento mucho más claro cuando se trata de bucles anidados.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de estas variables eliminadas. Si omite alguno, también debería ver los <strong>errores de la consola</strong> como: <code>Uncaught ReferenceError: $index is not defined</code> </p>
</div>
{% endraw%}

### `track-by` <sup>reemplazado</sup>

`track-by` ha sido reemplazado por` key`, que funciona como cualquier otro atributo: sin el prefijo `v-bind:` o `:`, se trata como una cadena literal. En la mayoría de los casos, desearía usar un enlace dinámico que espere una expresión completa en lugar de una clave. Por ejemplo, en lugar de:

`` `html
<div v-for="item in items" track-by="id">
`` `

Ahora escribirías:

`` `html
<div v-for="item in items" v-bind:key="item.id">
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>track-by</code> . </p>
</div>
{% endraw%}

### `v-for` Valores de rango <sup>cambiados</sup>

Anteriormente, `v-for =&quot; número en 10 &quot;` tendría `número` comenzando en 0 y terminando en 9. Ahora comienza en 1 y termina en 10.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Busque en la base de código la expresión regular <code>/\w+ in \d+/</code> . Donde sea que aparezca en una <code>v-for</code> , verifique si puede verse afectado. </p>
</div>
{% endraw%}

## apoyos

### `Coerce` Opción de Propulsión <sup>eliminada</sup>

Si desea forzar un prop, configure un valor computado local basado en él. Por ejemplo, en lugar de:

`` `js
accesorios: {
nombre de usuario: {
tipo: cadena,
coerce: función (valor) {
valor de retorno
.toLowerCase ()
.replace (/ \ s + /, &#39;-&#39;)
}
}
}
`` `

Podrías escribir:

`` `js
accesorios: {
nombre de usuario:
}
calculado: {
normalizedUsername: function () {
devolver este.usuario
.toLowerCase ()
.replace (/ \ s + /, &#39;-&#39;)
}
}
`` `

Hay algunas ventajas:

- Aún tiene acceso al valor original del prop.
- Te obligan a ser más explícito, al dar a tu valor bajo coacción un nombre que lo diferencia del valor pasado en la prop.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción de <code>coerce</code> . </p>
</div>
{% endraw%}

### `twoWay` Prop Opción <sup>eliminada</sup>

Los apoyos ahora están siempre en un solo sentido hacia abajo. Para producir efectos secundarios en el ámbito principal, un componente debe emitir explícitamente un evento en lugar de confiar en el enlace implícito. Para más información, ver:

- [Eventos de componentes personalizados] (components.html # Custom-Events)
- [Componentes de entrada personalizados] (components.html # Form-Input-Components-using-Custom-Events) (usando eventos de componente)
- [Gestión global del estado] (state-management.html)

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>twoWay</code> . </p>
</div>
{% endraw%}

### `.once` y` Modificadores .sync` en `v-bind` <sup>eliminan</sup>

Los apoyos ahora están siempre en un solo sentido hacia abajo. Para producir efectos secundarios en el ámbito principal, un componente debe emitir explícitamente un evento en lugar de confiar en el enlace implícito. Para más información, ver:

- [Eventos de componentes personalizados] (components.html # Custom-Events)
- [Componentes de entrada personalizados] (components.html # Form-Input-Components-using-Custom-Events) (usando eventos de componente)
- [Gestión global del estado] (state-management.html)

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">ayudante de la migración</a> de su base de código para encontrar ejemplos de la <code>.once</code> y <code>.sync</code> modificadores. </p>
</div>
{% endraw%}

### Prop Mutation <sup>deprecated</sup>

Mutar un prop localmente ahora se considera un anti-patrón, por ejemplo, declarar un prop y luego configurar &quot;this.myProp = &#39;someOtherValue&#39; &#39;en el componente. Debido al nuevo mecanismo de representación, siempre que el componente principal se vuelva a representar, los cambios locales del componente secundario se sobrescribirán.

La mayoría de los casos de uso de mutación de una hélice pueden reemplazarse por una de estas opciones:

- una propiedad de datos, con el prop utilizado para establecer su valor predeterminado
- una propiedad computada

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su suite o aplicación de prueba de extremo a extremo después de la actualización y busque <strong>advertencias de consola</strong> sobre mutaciones de prop. </p>
</div>
{% endraw%}

### Props en una instancia raíz <sup>reemplazada</sup>

En instancias de Vue raíz (es decir, instancias creadas con `new Vue ({...})`), debe usar `propsData` en lugar de` props`.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su conjunto de pruebas de extremo a extremo, si tiene uno. Las <strong>pruebas fallidas</strong> deben alertarle sobre el hecho de que los accesorios pasados a las instancias de raíz ya no funcionan. </p>
</div>
{% endraw%}

## propiedades calculadas

### `cache: false` en <sup>desuso</sup>

La invalidación de almacenamiento en caché de las propiedades calculadas se eliminará en futuras versiones principales de Vue. Reemplace cualquier propiedad computada sin caché con métodos, que tendrán el mismo resultado.

Por ejemplo:

`` `js
plantilla: &#39; <p> mensaje: {{timeMessage}} </p> &#39;,
calculado: {
mensaje de tiempo: {
caché: falso,
get: function () {
regresa Date.now () + this.message
}
}
}
`` `

O con métodos de componentes:

`` `js
modelo: &#39; <p> mensaje: {{getTimeMessage ()}} </p> &#39;,
métodos: {
getTimeMessage: function () {
regresa Date.now () + this.message
}
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la opción <code>cache: false</code> . </p>
</div>
{% endraw%}

## Directivas incorporadas

### Truthiness / Falsiness con `v-bind` <sup>cambió</sup>

Cuando se usa con `v-bind`, los únicos valores falsy son ahora:` null`, `undefined` y` false`. Esto significa &quot;0&quot; y las cadenas vacías se mostrarán como verdaderas. Entonces, por ejemplo, `v-bind: draggable =&quot; &#39;&#39; &quot;` se representará como `draggable =&quot; true &quot;`.

Para los atributos enumerados, además de los valores falsos anteriores, la cadena `&quot; false &quot;` también se representará como `attr =&quot; false &quot;`.

<p class="tip"> Tenga en cuenta que para otras directivas (por ejemplo, `v-if` y` v-show`), la veracidad normal de JavaScript sigue siendo válida. </p>

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su conjunto de pruebas de extremo a extremo, si tiene uno. Las <strong>pruebas fallidas</strong> deben avisarle de cualquier parte de su aplicación que pueda verse afectada por este cambio. </p>
</div>
{% endraw%}

### La escucha de eventos nativos en componentes con `v-on` <sup>cambió</sup>

Cuando se usa en un componente, `v-on` ahora solo escucha los eventos personalizados` $ emit`ted por ese componente. Para escuchar un evento DOM nativo en el elemento raíz, puede usar el modificador `.native`. Por ejemplo:

`` `html
<my-component v-on:click.native="doSomething"></my-component>
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su conjunto de pruebas de extremo a extremo, si tiene uno. Las <strong>pruebas fallidas</strong> deben avisarle de cualquier parte de su aplicación que pueda verse afectada por este cambio. </p>
</div>
{% endraw%}

### `debounce` Parámetro del parámetro para` v-model` <sup>eliminado</sup>

El anuncio se usa para limitar la frecuencia con la que ejecutamos solicitudes Ajax y otras operaciones costosas. El parámetro de atributo `debounce` de Vue para` v-model` hizo esto fácil para casos muy simples, pero en realidad se debió a __state updates__ en lugar de a las costosas operaciones en sí. Es una diferencia sutil, pero viene con limitaciones a medida que una aplicación crece.

Estas limitaciones se hacen evidentes al diseñar un indicador de búsqueda, como este, por ejemplo:

{% raw%}
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo" class="demo">
<input v-model="searchQuery" placeholder="Type something">
<strong>{{searchIndicator}}</strong>
</div>
<script>
nueva vista ({
el: &#39;# debounce-search-demo&#39;,
datos: {
consulta de busqueda: &#39;&#39;,
searchQueryIsDirty: false,
isCalculating: false
}
calculado: {
searchIndicator: function () {
if (this.isCalculating) {
volver &#39;⟳ Obteniendo nuevos resultados&#39;
} else if (this.searchQueryIsDirty) {
devuelve &#39;... escribiendo&#39;
} else {
devuelve &#39;✓ Hecho&#39;
}
}
}
reloj: {
searchQuery: function () {
this.searchQueryIsDirty = true
Esta Operación Expansiva ()
}
}
métodos: {
expensiveOperation: _.debounce (function () {
this.isCalculating = true
setTimeout (function () {
this.isCalculating = false
this.searchQueryIsDirty = false
} .bind (este), 1000)
}, 500)
}
})
</script>
{% endraw%}

Usando el atributo `debounce`, no habría manera de detectar el estado de&quot; Escritura &quot;, porque perdemos el acceso al estado en tiempo real de la entrada. Sin embargo, al desacoplar la función de rebote de Vue, solo podemos rebotar la operación que queremos limitar, eliminando los límites de las funciones que podemos desarrollar:

`` `html
<!--
Mediante el uso de la función de rebote de lodash u otro dedicado
biblioteca de utilidad, sabemos la implementación específica de rebote que
el uso será el mejor de su clase, y podemos usarlo EN CUALQUIER LUGAR. No solo
en nuestra plantilla
-&gt;
<script src="https://cdn.jsdelivr.net/lodash/4.13.1/lodash.js"></script>
<div id="debounce-search-demo">
<input v-model="searchQuery" placeholder="Type something">
<strong>{{searchIndicator}}</strong>
</div>
`` `

`` `js
nueva vista ({
el: &#39;# debounce-search-demo&#39;,
datos: {
consulta de busqueda: &#39;&#39;,
searchQueryIsDirty: false,
isCalculating: false
}
calculado: {
searchIndicator: function () {
if (this.isCalculating) {
volver &#39;⟳ Obteniendo nuevos resultados&#39;
} else if (this.searchQueryIsDirty) {
devuelve &#39;... escribiendo&#39;
} else {
devuelve &#39;✓ Hecho&#39;
}
}
}
reloj: {
searchQuery: function () {
this.searchQueryIsDirty = true
Esta Operación Expansiva ()
}
}
métodos: {
// Aquí es donde realmente pertenece el rebote.
expensiveOperation: _.debounce (function () {
this.isCalculating = true
setTimeout (function () {
this.isCalculating = false
this.searchQueryIsDirty = false
} .bind (este), 1000)
}, 500)
}
})
`` `

Otra ventaja de este enfoque es que habrá ocasiones en que el rebote no sea la función de ajuste correcta. Por ejemplo, cuando se accede a una API para buscar sugerencias, esperar para ofrecer sugerencias hasta después de que el usuario haya dejado de escribir durante un período de tiempo no es una experiencia ideal. Lo que probablemente quieras en cambio es una función __throttling__. Ahora, dado que ya está utilizando una biblioteca de utilidades como lodash, la refactorización para usar su función `throttle` toma solo unos segundos.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su código base para encontrar ejemplos del atributo de <code>debounce</code> . </p>
</div>
{% endraw%}

### Los atributos de `lazy` o` number` para `v-model` <sup>reemplazados</sup>

Los atributos param `lazy` y` number` ahora son modificadores, para dejar más claro lo que Eso significa en lugar de:

`` `html
<input v-model="name" lazy>
<input v-model="age" type="number" number>
`` `

Usted usaría:

`` `html
<input v-model.lazy="name">
<input v-model.number="age" type="number">
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de estos atributos param. </p>
</div>
{% endraw%}

### `value` Atributo con` v-model` <sup>eliminado</sup>

`v-model` ya no se preocupa por el valor inicial de un atributo` value` en línea. Para la previsibilidad, en cambio, siempre tratará los datos de la instancia de Vue como la fuente de la verdad.

Eso significa este elemento:

`` `html
<input v-model="text" value="foo">
`` `

respaldado por estos datos:

`` `js
datos: {
texto: &#39;bar&#39;
}
`` `

<html> se representará con un valor de &quot;barra&quot; en lugar de &quot;foo&quot;. Lo mismo vale para un ` <textarea> `Con contenido existente. En lugar de: &lt;/html&gt;

`` `html
<html><textarea v-model="text"> &lt;/html&gt;
Hola Mundo
</textarea>
`` `

Debe asegurarse de que su valor inicial para `texto` sea&quot; hola mundo &quot;.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su suite o aplicación de prueba de extremo a extremo después de la actualización y busque <strong>advertencias de consola</strong> sobre los atributos de valor en línea con <code>v-model</code> . </p>
</div>
{% endraw%}

### `v-model` con` v-for` Valores primitivos iterados <sup>eliminados</sup>

Casos como este ya no funcionan:

`` `html
<input v-for="str in strings" v-model="str">
`` `

La razón es que este es el equivalente de JavaScript que el ` <input> `compilaría para:

`` `js
strings.map (function (str) {
devuelve createElement (&#39;input&#39;, ...)
})
`` `

Como puede ver, el enlace bidireccional del `v-model` no tiene sentido aquí. Establecer `str` en otro valor en la función iterador no hará nada porque es solo una variable local en el alcance de la función.

En su lugar, debe usar una matriz de __objects__ para que `v-model` pueda actualizar el campo en el objeto. Por ejemplo:

`` `html
<input v-for="obj in objects" v-model="obj.str">
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su suite de prueba, si tiene uno. Las <strong>pruebas fallidas</strong> deben avisarle de cualquier parte de su aplicación que pueda verse afectada por este cambio. </p>
</div>
{% endraw%}

### `v-bind: style` con la sintaxis de objetos y`! important` <sup>eliminados</sup>

Esto ya no funcionará:

`` `html
<p v-bind:style="{ color: myColor + ' !important' }"> Hola </p>
`` `

Si realmente necesita anular otro `! Important`, debe usar la sintaxis de cadena:

`` `html
<p v-bind:style="'color: ' + myColor + ' !important'"> Hola </p>
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de enlaces de estilo con <code>!important</code> en los objetos. </p>
</div>
{% endraw%}

### `v-el` y` v-ref` <sup>reemplazados</sup>

Para simplificar, `v-el` y` v-ref` se han fusionado en el atributo `ref`, accesible en una instancia de componente a través de` $ refs`. Eso significa que `v-el: my-element` se convertiría en` ref = &quot;myElement&quot; `y` v-ref: my-component` se convertiría en `ref =&quot; myComponent &quot;`. Cuando se usa en un elemento normal, el `ref` será el elemento DOM, y cuando se usa en un componente, el` ref` será la instancia del componente.

Como `v-ref` ya no es una directiva, sino un atributo especial, también se puede definir dinámicamente. Esto es especialmente útil en combinación con `v-for`. Por ejemplo:

`` `html
<p v-for="item in items" v-bind:ref="'item' + item.id"></p>
`` `

Anteriormente, `v-el` /` v-ref` combinado con `v-for` produciría un conjunto de elementos / componentes, porque no había manera de dar a cada elemento un nombre único. Aún puedes lograr este comportamiento dando a cada elemento la misma `ref`:

`` `html
<p v-for="item in items" ref="items"></p>
`` `

A diferencia de en 1.x, estos $ refs no son reactivos, porque se registran / actualizan durante el proceso de renderizado. Hacerlos reactivos requeriría renders duplicados para cada cambio.

Por otro lado, `$ refs` está diseñado principalmente para el acceso programático en JavaScript; no se recomienda confiar en ellos en las plantillas, ya que eso significaría referirse al estado que no pertenece a la instancia en sí. Esto violaría el modelo de vista basado en datos de Vue.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>v-el</code> y <code>v-ref</code> . </p>
</div>
{% endraw%}

### `v-else` con` v-show` <sup>eliminado</sup>

`v-else` ya no funciona con` v-show`. Use `v-if` con una expresión de negación en su lugar. Por ejemplo, en lugar de:

`` `html
<p v-if="foo"> Foo </p>
<p v-else v-show="bar"> No foo, pero bar </p>
`` `

Puedes usar:

`` `html
<p v-if="foo"> Foo </p>
<p v-if="!foo && bar"> No foo, pero bar </p>
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>v-else</code> con <code>v-show</code> . </p>
</div>
{% endraw%}

## Directivas personalizadas <sup>simplificadas</sup>

Las directivas tienen un alcance de responsabilidad muy reducido: ahora solo se utilizan para aplicar manipulaciones directas de DOM de bajo nivel. En la mayoría de los casos, debería preferir usar componentes como la abstracción principal de reutilización de código.

Algunas de las diferencias más notables incluyen:

- Las directivas ya no tienen instancias. Esto significa que no hay más `this` dentro de los enganches directivos. En su lugar, reciben todo lo que puedan necesitar como argumentos. Si realmente debe persistir el estado en los ganchos, puede hacerlo en `el`.
- Se han eliminado todas las opciones como `acceptStatement`,` deep`, `prioridad`, etc. Para reemplazar las directivas `twoWay`, vea [este ejemplo] (# Reemplazo de filtros de dos vías).
- Algunos de los ganchos actuales tienen un comportamiento diferente y también hay un par de nuevos ganchos.

Afortunadamente, como las nuevas directivas son mucho más simples, puede dominarlas más fácilmente. Lea la nueva [Guía de directivas personalizadas] (custom-managers.html) para obtener más información.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de directivas definidas. El ayudante los marcará a todos, ya que es probable que en la mayoría de los casos desee refactorizar un componente. </p>
</div>
{% endraw%}

### Directiva `modificador .literal` <sup>retira</sup>

El modificador `.literal` se ha eliminado, ya que se puede lograr el mismo proporcionando un literal de cadena como valor.

Por ejemplo, puede actualizar:

`` `js
<p v-my-directive.literal="foo bar baz"></p>
`` `

a:

`` `html
<p v-my-directive="'foo bar baz'"></p>
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del modificador `.literal` en una directiva. </p>
</div>
{% endraw%}

## Transiciones

### `transición` Atributo <sup>reemplazado</sup>

El sistema de transición de Vue ha cambiado bastante drásticamente y ahora usa ` <transition> `y` <transition-group> Elementos de envoltura, en lugar del atributo de transición. Se recomienda leer la nueva [Guía de transiciones] (transitions.html) para obtener más información.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del atributo de <code>transition</code> . </p>
</div>
{% endraw%}

### `Vue.transition` para transiciones reutilizables <sup>reemplazadas</sup>

Con el nuevo sistema de transición, ahora puede [usar componentes para transiciones reutilizables] (transitions.html # Reusable-Transitions).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.transition</code> . </p>
</div>
{% endraw%}

### Transition `stagger` Atributo <sup>eliminado</sup>

Si necesita escalonar las transiciones de la lista, puede controlar el tiempo configurando y accediendo a un &#39;índice de datos&#39; (o atributo similar) en un elemento. Vea [un ejemplo aquí] (transitions.html # Staggering-List-Transitions).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del atributo de <code>transition</code> . Durante su actualización, también puede hacer la transición (es decir, con mucha intención) a la nueva estrategia asombrosa. </p>
</div>
{% endraw%}

## Eventos

Opción ### `events` <sup>eliminada</sup>

La opción `eventos` ha sido eliminada. Los manejadores de eventos ahora deben estar registrados en el gancho `created` en su lugar. Echa un vistazo a la guía de migración [`$ dispatch` y` $ broadcast`] (# dispatch-and-broadcast-replacement) para un ejemplo detallado.

### `Vue.directive (&#39;on&#39;). KeyCodes` <sup>reemplazado</sup>

La nueva forma más concisa de configurar `keyCodes` es a través de` Vue.config.keyCodes`. Por ejemplo:

`` `js
// habilitar v-on: keyup.f1
View.config.keyCodes.f1 = 112
`` `
{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la antigua sintaxis de configuración de <code>keyCode</code> . </p>
</div>
{% endraw%}

### `$ dispatch` y` $ broadcast` <sup>reemplazados</sup>

`$ dispatch` y` $ broadcast` se han eliminado en favor de una comunicación más explícita de componentes cruzados y soluciones de administración de estado más sostenibles, como [Vuex] (https://github.com/vuejs/vuex).

El problema es que los flujos de eventos que dependen de la estructura de árbol de un componente pueden ser difíciles de razonar y son muy frágiles cuando el árbol crece. No se escalan bien y solo te preparan para el dolor más tarde. `$ dispatch` y` $ broadcast` tampoco resuelven la comunicación entre los componentes hermanos.

Uno de los usos más comunes de estos métodos es comunicarse entre un padre y sus hijos directos. En estos casos, realmente puede [escuchar un `$ emit` de un niño con` v-on`] (components.html # Form-Input-Components-using-Custom-Events). Esto le permite mantener la conveniencia de los eventos con una explicación adicional.

Sin embargo, cuando se comunica entre descendientes / ancestros distantes, `$ emit` no lo ayudará. En su lugar, la actualización más simple posible sería utilizar un centro de eventos centralizado. Esto tiene el beneficio adicional de permitirle comunicarse entre componentes sin importar dónde se encuentren en el árbol de componentes, ¡incluso entre hermanos! Debido a que las instancias de Vue implementan una interfaz de emisores de eventos, puede usar una instancia de Vue vacía para este propósito.

Por ejemplo, digamos que tenemos una aplicación para todo estructurado de esta manera:

`` `
Todos
├─ NuevoTodoEntrada
└─ Todo
└─ DeleteTodoButton
`` `

Podríamos gestionar la comunicación entre componentes con este único centro de eventos:

`` `js
// Este es el centro de eventos que usaremos en cada
// componente para comunicarse entre ellos.
var eventHub = nueva vista ()
`` `

Luego, en nuestros componentes, podemos usar `$ emit`,` $ on`, `$ off` para emitir eventos, escuchar eventos y limpiar escuchas de eventos, respectivamente:

`` `js
// NewTodoInput
// ...
métodos: {
addTodo: function () {
eventHub. $ emit (&#39;add-todo&#39;, {text: this.newTodoText})
this.newTodoText = &#39;&#39;
}
}
`` `

`` `js
// DeleteTodoButton
// ...
métodos: {
deleteTodo: function (id) {
eventHub. $ emit (&#39;delete-todo&#39;, id)
}
}
`` `

`` `js
// Todos
// ...
creado: function () {
eventHub. $ on (&#39;add-todo&#39;, this.addTodo)
eventHub. $ on (&#39;delete-todo&#39;, this.deleteTodo)
}
// Es bueno limpiar a los oyentes del evento antes
// se destruye un componente.
beforeDestroy: function () {
eventHub. $ off (&#39;add-todo&#39;, this.addTodo)
eventHub. $ off (&#39;delete-todo&#39;, this.deleteTodo)
}
métodos: {
addTodo: function (newTodo) {
this.todos.push (newTodo)
}
deleteTodo: function (todoId) {
this.todos = this.todos.filter (function (todo) {
volver todo.id! == todoId
})
}
}
`` `

Este patrón puede servir como un reemplazo para `$ dispatch` y` $ broadcast` en escenarios simples, pero para casos más complejos, se recomienda usar una capa de administración de estado dedicada como [Vuex] (https://github.com/ vuejs / vuex).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>$dispatch</code> y <code>$broadcast</code> . </p>
</div>
{% endraw%}

## Filtros

### Filtros fuera de las interpolaciones de texto <sup>eliminados</sup>

Los filtros ahora solo se pueden usar dentro de las interpolaciones de texto (etiquetas `{% raw%} {{}} {% endraw%}`). En el pasado, hemos encontrado que el uso de filtros en directivas como `v-model`,` v-on`, etc. condujo a una mayor complejidad que a la conveniencia. Para el filtrado de listas en `v-for`, también es mejor mover esa lógica a JavaScript como propiedades computadas, para que pueda reutilizarse en todo el componente.

En general, siempre que se pueda lograr algo en JavaScript simple, queremos evitar la introducción de una sintaxis especial, como filtros, para atender la misma preocupación. Aquí es cómo puede reemplazar los filtros de directiva incorporados de Vue:

#### Reemplazando el filtro `debounce`

En lugar de:

`` `html
<input v-on:keyup="doStuff | debounce 500">
`` `

`` `js
métodos: {
doStuff: function () {
// ...
}
}
`` `

Utilice [debash de lodash] (https://lodash.com/docs/4.15.0#debounce) (o posiblemente [`throttle`] (https://lodash.com/docs/4.15.0#throttle)) para limitar directamente llamando al método caro. Puedes lograr lo mismo que arriba como esta:

`` `html
<input v-on:keyup="doStuff">
`` `

`` `js
métodos: {
doStuff: _.debounce (function () {
// ...
}, 500)
}
`` `

Para obtener más información sobre las ventajas de esta estrategia, vea [el ejemplo aquí con `v-model`] (# debounce-Param-Attribute-for-v-model-remove).

#### Reemplazando el filtro `limitBy`

En lugar de:

`` `html
<p v-for="item in items | limitBy 10"> {{ ít }} </p>
`` `

Use el método [`.slice`] incorporado de JavaScript (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#Examples) en una propiedad computada:

`` `html
<p v-for="item in filteredItems"> {{ ít }} </p>
`` `

`` `js
calculado: {
FilterItems: function () {
devuelve this.items.slice (0, 10)
}
}
`` `

#### Sustituyendo el filtro `filterBy`

En lugar de:

`` `html
<p v-for="user in users | filterBy searchQuery in 'name'"> {{nombre de usuario}} </p>
`` `

Use el método [`.filter`] incorporado de JavaScript (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter#Examples) en una propiedad computada:

`` `html
<p v-for="user in filteredUsers"> {{nombre de usuario}} </p>
`` `

`` `js
calculado: {
FiltrosUsuarios: función () {
var self = esto
return self.users.filter (función (usuario) {
devuelve user.name.indexOf (self.searchQuery)! == -1
})
}
}
`` `

El &quot;.filter&quot; nativo de JavaScript también puede administrar operaciones de filtrado mucho más complejas, porque tiene acceso a todo el poder de JavaScript dentro de las propiedades computadas. Por ejemplo, si desea encontrar a todos los usuarios activos y hacer coincidir sin distinción de mayúsculas y minúsculas con su nombre y correo electrónico:

`` `js
var self = esto
self.users.filter (función (usuario) {
var searchRegex = new RegExp (self.searchQuery, &#39;i&#39;)
devuelve user.isActive &amp;&amp; (
searchRegex.test (usuario.nombre) ||
searchRegex.test (user.email)
)
})
`` `

#### Reemplazando el filtro `orderBy`

En lugar de:

`` `html
<p v-for="user in users | orderBy 'name'"> {{nombre de usuario}} </p>
`` `

Utilice [orderBy` de lodash] (https://lodash.com/docs/4.15.0#orderBy) (o posiblemente [`sortBy`] (https://lodash.com/docs/4.15.0#sortBy)) en una propiedad calculada:

`` `html
<p v-for="user in orderedUsers"> {{nombre de usuario}} </p>
`` `

`` `js
calculado: {
ordenadosUsuarios: función () {
return _.orderBy (this.users, &#39;name&#39;)
}
}
`` `

Incluso puedes ordenar por múltiples columnas:

`` `js
_.orderBy (this.users, [&#39;name&#39;, &#39;last_login&#39;], [&#39;asc&#39;, &#39;desc&#39;])
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de filtros que se utilizan dentro de las directivas. Si <strong>fallas,</strong> también deberías ver los <strong>errores de la consola</strong> . </p>
</div>
{% endraw%}

### La sintaxis del argumento del filtro <sup>cambió</sup>

La sintaxis de los filtros para los argumentos ahora se alinea mejor con la invocación de la función JavaScript. Así que en lugar de tomar argumentos delimitados por el espacio:

`` `html
<p> {{fecha | formatDate &#39;YY-MM-DD&#39; timeZone}} </p>
`` `

Rodeamos los argumentos con paréntesis y delimitamos los argumentos con comas:

`` `html
<p> {{fecha | formatDate (&#39;YY-MM-DD&#39;, timeZone)}} </p>
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de la sintaxis del filtro anterior. Si <strong>fallas,</strong> también deberías ver los <strong>errores de la consola</strong> . </p>
</div>
{% endraw%}

### Filtros de texto incorporados <sup>eliminados</sup>

Aunque los filtros dentro de las interpolaciones de texto todavía están permitidos, todos los filtros se han eliminado. En cambio, se recomienda usar bibliotecas más especializadas para resolver problemas en cada dominio (por ejemplo, [`date-fns`] (https://date-fns.org/) para formatear fechas y [` accounting`] (http: // openexchangerates.github.io/accounting.js/) para las monedas).

Para cada uno de los filtros de texto integrados de Vue, veremos cómo puede reemplazarlos a continuación. El código de ejemplo podría existir en funciones de ayuda personalizadas, métodos o propiedades computadas.

#### Sustituyendo el filtro `json`

En realidad, ya no es necesario hacerlo para la depuración, ya que Vue formateará muy bien la salida automáticamente, ya sea una cadena, un número, una matriz o un objeto plano. Sin embargo, si desea la misma funcionalidad que `JSON.stringify` de JavaScript, puede usarla en un método o propiedad computada.

#### Sustituyendo el filtro `capitalize`

`` `js
text [0] .toUpperCase () + text.slice (1)
`` `

#### Sustituyendo el filtro `mayúscula`

`` `js
text.toUpperCase ()
`` `

#### Sustituyendo el filtro `minúscula`

`` `js
text.toLowerCase ()
`` `

#### Sustituyendo el filtro `pluralize`

El paquete [pluralize] (https://www.npmjs.com/package/pluralize) en NPM sirve muy bien para este propósito, pero si solo desea pluralizar una palabra específica o desea obtener un resultado especial para casos como `0` entonces también puede definir fácilmente sus propias funciones de pluralización. Por ejemplo:

`` `js
función pluralizeKnife (cuenta) {
si (cuenta === 0) {
devuelve &#39;no cuchillos&#39;
} else if (cuenta === 1) {
devuelve &#39;1 cuchillo&#39;
} else {
cuenta regresiva + &#39;cuchillos&#39;
}
}
`` `

#### Sustituyendo el filtro `moneda`

Para una implementación muy ingenua, podrías hacer algo como esto:

`` `js
&#39;$&#39; + precio.a Fijado (2)
`` `

Sin embargo, en muchos casos, aún tendrá un comportamiento extraño (por ejemplo, `0.035.toFixed (2)` redondea a `0.04`, pero` 0.045` redondea a `0.04`). Para solucionar estos problemas, puede usar la biblioteca [`accounting`] (http://openexchangerates.github.io/accounting.js/) para formatear las monedas de manera más confiable.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de los filtros de texto obsoletos. Si <strong>fallas,</strong> también deberías ver los <strong>errores de la consola</strong> . </p>
</div>
{% endraw%}

### Filtros de dos vías <sup>reemplazados</sup>

Algunos usuarios han disfrutado utilizando filtros de dos vías con `v-model` para crear entradas interesantes con muy poco código. Sin embargo, aunque aparentemente simple, los filtros bidireccionales también pueden ocultar una gran complejidad e incluso alentar un UX deficiente retrasando las actualizaciones de estado. En su lugar, los componentes que envuelven una entrada se recomiendan como una forma más explícita y rica en características de crear entradas personalizadas.

Como ejemplo, ahora veremos la migración de un filtro de moneda bidireccional:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/6744xnjk/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

En su mayoría funciona bien, pero las actualizaciones de estado retrasadas pueden causar un comportamiento extraño. Por ejemplo, haga clic en la pestaña `Resultado` e intente ingresar` 9.999` en una de esas entradas. Cuando la entrada pierde el enfoque, su valor se actualizará a `$ 10.00`. Sin embargo, al mirar el total calculado, verá que `9.999` es lo que está almacenado en nuestros datos. ¡La versión de realidad que el usuario ve no está sincronizada!

Para comenzar la transición hacia una solución más robusta usando Vue 2.0, primero envolvamos este filtro en un nuevo ` <currency-input> `componente:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/943zfbsh/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Esto nos permite agregar un comportamiento que un filtro por sí solo no puede encapsular, como seleccionar el contenido de una entrada en el foco. Ahora el siguiente paso será extraer la lógica empresarial del filtro. A continuación, sacamos todo en un [objeto currencyValidator` externo] (https://gist.github.com/chrisvfritz/5f0a639590d6e648933416f90ba7ae4e):

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/9c32kev2/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Este aumento de la modularidad no solo facilita la migración a Vue 2, sino que también permite que el análisis y el formato de la moneda sean:

- Unidad probada aislada de su código Vue
- utilizado por otras partes de su aplicación, como para validar la carga útil a un punto final de API

Habiendo extraído este validador, también lo hemos construido más cómodamente en una solución más robusta. Las peculiaridades del estado han sido eliminadas y, en realidad, es imposible para los usuarios ingresar algo incorrecto, similar a lo que intenta hacer la entrada del número nativo del navegador.

Sin embargo, aún estamos limitados por los filtros y por Vue 1.0 en general, así que completemos la actualización a Vue 2.0:

<iframe width="100%" height="300" src="https://jsfiddle.net/chrisvfritz/1oqjojjx/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Usted puede notar que:

- Cada aspecto de nuestra entrada es más explícito, usando ganchos de ciclo de vida y eventos DOM en lugar del comportamiento oculto de los filtros de dos vías.
- Ahora podemos usar `v-model` directamente en nuestras entradas personalizadas, lo que no solo es más consistente con las entradas normales, sino que también significa que nuestro componente es compatible con Vuex.
- Dado que ya no estamos utilizando las opciones de filtro que requieren que se devuelva un valor, nuestro trabajo con la moneda podría realizarse de forma asíncrona. Eso significa que si tuviéramos muchas aplicaciones que tuvieran que trabajar con monedas, podríamos refactorizar fácilmente esta lógica en un microservicio compartido.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de filtros utilizados en directivas como <code>v-model</code> . Si <strong>fallas,</strong> también deberías ver los <strong>errores de la consola</strong> . </p>
</div>
{% endraw%}

## Slots

### Se <sup>eliminaron las</sup> ranuras duplicadas

Ya no se soporta tener ` <slot> `s con el mismo nombre en la misma plantilla. Cuando se representa una ranura, se &quot;usa&quot; y no se puede representar en ninguna otra parte del mismo árbol de representación. Si debe representar el mismo contenido en varios lugares, pase ese contenido como prop.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su suite o aplicación de prueba de extremo a extremo después de la actualización y busque las <strong>advertencias de la consola</strong> acerca de las ranuras duplicadas <code>v-model</code> . </p>
</div>
{% endraw%}

### `slot` Atribute Styling <sup>eliminado</sup>

Contenido insertado a través de llamado ` <slot> `Ya no conserva el atributo` slot`. Utilice un elemento de envoltura para personalizarlos o, para casos de uso avanzados, modifique el contenido insertado mediante programación utilizando [funciones de representación] (render-function.html).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar los selectores de CSS que se dirigen a las ranuras con nombre (por ejemplo, <code>[slot=&quot;my-slot-name&quot;]</code> ). </p>
</div>
{% endraw%}

## Atributos especiales

### `keep-alive` Atributo <sup>reemplazado</sup>

`keep-alive` ya no es un atributo especial, sino un componente de envoltura, similar a` <transition> `. Por ejemplo:

`` `html
<keep-alive>
<component v-bind:is="view"></component>
</keep-alive>
`` `

Esto hace posible utilizar ` <keep-alive> `en múltiples hijos condicionales:

`` `html
<keep-alive>
<todo-list v-if="todos.length > 0"></todo-list>
<no-todos-gif v-else></no-todos-gif>
</keep-alive>
`` `

<p class="tip"> Cuando <keep-alive> `tiene varios hijos, deberían eventualmente evaluar a un solo niño. Cualquier otro niño que no sea el primero será ignorado. </p>

Cuando se usa junto con ` <transition> `, asegúrese de anidar en el interior:

`` `html
<transition>
<keep-alive>
<component v-bind:is="view"></component>
</keep-alive>
</transition>
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar <code>keep-alive</code> atributos de <code>keep-alive</code> . </p>
</div>
{% endraw%}

## Interpolación

### Interpolación dentro de los atributos <sup>eliminados</sup>

La interpolación dentro de los atributos ya no es válida. Por ejemplo:

`` `html
<button class="btn btn-{{ size }}"></button>
`` `

Debe ser actualizado para usar una expresión en línea:

`` `html
<button v-bind:class="'btn btn-' + size"></button>
`` `

O una propiedad de datos / computada:

`` `html
<button v-bind:class="buttonClasses"></button>
`` `

`` `js
calculado: {
buttonClasses: function () {
devuelve &#39;btn btn-&#39; + tamaño
}
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de interpolación utilizados dentro de los atributos. </p>
</div>
{% endraw%}

### Interpolación HTML <sup>eliminada</sup>

Se han eliminado las interpolaciones HTML (`{% raw%} {{{foo}}} {% endraw%}`) en favor de la [directiva v `html`] (../ api / # v-html).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar interpolaciones HTML. </p>
</div>
{% endraw%}

### <sup>Reemplazos de</sup> una sola vez <sup>reemplazados</sup>

Los enlaces de una vez (`{% raw%} {{* foo}} {% endraw%}`) han sido reemplazados por la nueva [directiva &#39;v-once&#39;] (../ api / # v-once).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar enlaces de una sola vez. </p>
</div>
{% endraw%}

## Reactividad

### `vm. $ watch` <sup>cambiado</sup>

Los observadores creados a través de `vm. $ Watch` ahora se disparan antes que los componentes asociados. Esto le da la oportunidad de actualizar aún más el estado antes de que se vuelva a enviar el componente, evitando así actualizaciones innecesarias. Por ejemplo, puede ver una prop de componente y actualizar los datos propios del componente cuando la proposición cambie.

Si anteriormente confiaba en `vm. $ Watch` para hacer algo con el DOM después de la actualización de un componente, puede hacerlo en el gancho del ciclo de vida` updated`.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute su conjunto de pruebas de extremo a extremo, si tiene uno. Las <strong>pruebas fallidas</strong> deben alertarle sobre el hecho de que un observador confía en el comportamiento anterior. </p>
</div>
{% endraw%}

### `vm. $ set` <sup>cambiado</sup>

`vm. $ set` ahora es un alias para [` Vue.set`] (../ api / # Vue-set).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del uso obsoleto. </p>
</div>
{% endraw%}

### `vm. $ delete` <sup>cambiado</sup>

`vm. $ delete` ahora es un alias para [` Vue.delete`] (../ api / # Vue-delete).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos del uso obsoleto. </p>
</div>
{% endraw%}

### `Array.prototype. $ Set` <sup>eliminado</sup>

Utilice `Vue.set` en su lugar.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>.$set</code> en una matriz. Si pierde alguno, debería ver los <strong>errores</strong> de la <strong>consola</strong> del método que falta. </p>
</div>
{% endraw%}

### `Array.prototype. $ Remove` <sup>eliminado</sup>

Utilice `Array.prototype.splice` en su lugar. Por ejemplo:

`` `js
métodos: {
  removeTodo: function (todo) {
var index = this.todos.indexOf (all)
this.todos.splice (índice, 1)
}
}
`` `

O mejor aún, pase los métodos de eliminación un índice:

`` `js
métodos: {
removeTodo: función (índice) {
this.todos.splice (índice, 1)
}
}
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>.$remove</code> en una matriz. Si pierde alguno, debería ver los <strong>errores</strong> de la <strong>consola</strong> del método que falta. </p>
</div>
{% endraw%}

### `Vue.set` y` Vue.delete` en la vista Instancias <sup>eliminadas</sup>

`Vue.set` y` Vue.delete` ya no pueden funcionar en instancias de Vue. Ahora es obligatorio declarar correctamente todas las propiedades reactivas de nivel superior en la opción de datos. Si desea eliminar las propiedades en una instancia de Vue o su `$ data`, configúrelo en nulo.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.set</code> o <code>Vue.delete</code> en una instancia de Vue. Si fallas, dispararán las <strong>advertencias de la consola</strong> . </p>
</div>
{% endraw%}

### Reemplazando `vm. $ Data` <sup>eliminado</sup>

Ahora está prohibido reemplazar la raíz de datos de una instancia del componente. Esto evita algunos casos de borde en el sistema de reactividad y hace que el estado del componente sea más predecible (especialmente con los sistemas de verificación de tipo).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de sobrescribir <code>vm.$data</code> . Si fallas, <strong>se</strong> emitirán <strong>avisos de consola</strong> . </p>
</div>
{% endraw%}

### `vm. $ get` <sup>eliminado</sup>

En su lugar, recuperar datos reactivos directamente.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$get</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

## Métodos de instancia enfocados en DOM

### `vm. $ appendTo` <sup>eliminado</sup>

Utilice la API de DOM nativa:

`` `js
myElement.appendChild (vm. $ el)
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$appendTo</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

### `vm. $ before` <sup>eliminado</sup>

Utilice la API de DOM nativa:

`` `js
myElement.parentNode.insertBefore (vm. $ el, myElement)
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$before</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

### `vm. $ after` <sup>eliminado</sup>

Utilice la API de DOM nativa:

`` `js
myElement.parentNode.insertBefore (vm. $ el, myElement.nextSibling)
`` `

O si `myElement` es el último hijo:

`` `js
myElement.parentNode.appendChild (vm. $ el)
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$after</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

### `vm. $ remove` <sup>eliminado</sup>

Utilice la API de DOM nativa:

`` `js
vm.$el.remove()
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$remove</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

Métodos de instancia meta

### `vm. $ eval` <sup>eliminado</sup>

Sin uso real. Si de alguna manera confía en esta característica y no está seguro de cómo solucionarla, publique en [el foro] (https://forum.vuejs.org/) para obtener ideas.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$eval</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

### `vm. $ interpolate` <sup>eliminado</sup>

Sin uso real. Si de alguna manera confía en esta característica y no está seguro de cómo solucionarla, publique en [el foro] (https://forum.vuejs.org/) para obtener ideas.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$interpolate</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

### `vm. $ log` <sup>eliminado</sup>

Use [Vue Devtools] (https://github.com/vuejs/vue-devtools) para obtener una experiencia de depuración óptima.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>vm.$log</code> . Si <strong>fallas</strong> , verás <strong>errores en la consola</strong> . </p>
</div>
{% endraw%}

## Opciones de DOM de instancia

### `reemplazar: falso` <sup>eliminado</sup>

Los componentes ahora siempre reemplazan el elemento al que están vinculados. Para simular el comportamiento de `replace: false`, puedes envolver tu componente raíz con un elemento similar al que estás reemplazando. Por ejemplo:

`` `js
nueva vista ({
  el: '#app',
modelo: &#39; <div id="app"> ... </div> &#39;
})
`` `

O con una función de render:

`` `js
nueva vista ({
  el: '#app',
render: función (h) {
h (&#39;div&#39;, {
attrs: {
ID: &#39;aplicación&#39;,
}
}, / * ... * /)
}
})
`` `

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>replace: false</code> . </p>
</div>
{% endraw%}

## Configuración global

### `Vue.config.debug` <sup>eliminado</sup>

Ya no es necesario, ya que las advertencias vienen con rastreos de pila por defecto ahora.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.config.debug</code> . </p>
</div>
{% endraw%}

### `Vue.config.async` <sup>eliminado</sup>

Ahora se requiere Async para el rendimiento de representación.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.config.async</code> . </p>
</div>
{% endraw%}

### `Vue.config.delimiters` <sup>reemplazado</sup>

Esto se ha reelaborado como [opción a nivel de componente] (../ api / # delimiters). Esto le permite usar delimitadores alternativos dentro de su aplicación sin romper los componentes de terceros.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.config.delimiters</code> . </p>
</div>
{% endraw%}

### `Vue.config.unsafeDelimiters` <sup>eliminado</sup>

La interpolación de HTML ha sido [eliminada a favor de `v-html`] (# HTML-Interpolation-eliminar).

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.config.unsafeDelimiters</code> . Después de esto, el ayudante también encontrará instancias de interpolación de HTML para que pueda reemplazarlos con `v-html`. </p>
</div>
{% endraw%}

## API global

### `Vue.extend` con` el` <sup>eliminado</sup>

La opción el ya no se puede usar en `Vue.extend`. Sólo es válido como una opción de creación de instancia.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecutar su banco de pruebas de extremo a extremo o la aplicación después de la actualización y busque <strong>las advertencias</strong> acerca de la <strong>consola</strong> <code>el</code> opción con <code>Vue.extend</code> . </p>
</div>
{% endraw%}

### `Vue.elementDirective` <sup>eliminado</sup>

Utilice componentes en su lugar.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.elementDirective</code> . </p>
</div>
{% endraw%}

### `Vue.partial` <sup>eliminado</sup>

Los parciales se han eliminado en favor de un flujo de datos más explícito entre los componentes, utilizando accesorios. A menos que esté usando un parcial en un área de rendimiento crítico, la recomendación es usar un [componente normal] (components.html) en su lugar. Si estaba vinculando dinámicamente el `nombre` de un parcial, puede usar un [componente dinámico] (components.html # Dynamic-Components).

Si está utilizando parciales en una parte crítica de su aplicación, debería actualizar a [componentes funcionales] (render-function.html # Functional-Components). Deben estar en un archivo plano JS / JSX (en lugar de en un archivo `.vue`) y no tienen estado ni instancias, como los parciales. Esto hace que el renderizado sea extremadamente rápido.

Un beneficio de los componentes funcionales sobre los parciales es que pueden ser mucho más dinámicos, porque le otorgan acceso a todo el poder de JavaScript. Sin embargo, hay un costo para este poder. Si nunca ha usado un marco de componente con funciones de renderizado, es posible que demoren un poco más en aprender.

{% raw%}
<div class="upgrade-path">
<h4> Ruta de actualización </h4>
<p> Ejecute el <a href="https://github.com/vuejs/vue-migration-helper">asistente de migración</a> en su base de código para encontrar ejemplos de <code>Vue.partial</code> . </p>
</div>
{% endraw%}

