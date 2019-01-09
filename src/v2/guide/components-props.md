---
Título: Props
tipo: guía
orden: 102
---

&gt; Esta página asume que ya ha leído los [Conceptos básicos de los componentes] (components.html). Lea eso primero si es nuevo en componentes.

## Prop Casing (camelCase vs kebab-case)

Los nombres de atributos HTML no distinguen entre mayúsculas y minúsculas, por lo que los navegadores interpretarán cualquier carácter en mayúscula como en minúscula. Eso significa que cuando estás usando plantillas en DOM, los nombres de propiedades de camelCased necesitan usar sus equivalentes en kebab-encajonado (delimitados por guiones)

`` `js
View.component (&#39;blog-post&#39;, {
// camelCase en JavaScript
objetos: [&#39;postTitle&#39;],
modelo: &#39; <h3> {{ título de la entrada }} </h3> &#39;
})
`` `

`` `html
<!-- kebab-case in HTML -->
<blog-post post-title="hello!"></blog-post>
`` `

Nuevamente, si está utilizando plantillas de cadena, esta limitación no se aplica.

## tipos de prop

Hasta ahora, solo hemos visto los accesorios listados como una serie de cadenas:

`` `js
accesorios: [&#39;title&#39;, &#39;likes&#39;, &#39;isPublished&#39;, &#39;commentIds&#39;, &#39;author&#39;]
`` `

Sin embargo, por lo general, querrá que cada prop sea un tipo específico de valor. En estos casos, puede enumerar los accesorios como un objeto, donde los nombres y valores de las propiedades contienen los nombres y tipos de los accesorios, respectivamente:

`` `js
accesorios: {
título: cadena,
me gusta: número,
ispublicado: booleano,
commentIds: Array,
autor: objeto
}
`` `

Esto no solo documenta su componente, sino que también advertirá a los usuarios en la consola de JavaScript del navegador si pasan el tipo incorrecto. Aprenderá mucho más sobre [verificaciones de tipos y otras validaciones de propiedades] (# Validación de Prop) más adelante en esta página.

## Pasando apoyos estáticos o dinámicos

Hasta ahora, has visto que los accesorios pasaron un valor estático, como en:

`` `html
<blog-post title="Mi viaje con vue"></blog-post>
`` `

También has visto accesorios asignados dinámicamente con `v-bind`, como en:

`` `html
<!-- Dynamically assign the value of a variable -->
<blog-post v-bind:title="post.title"></blog-post>

<!-- Dynamically assign the value of a complex expression -->
<blog-post v-bind:title="post.title + ' by ' + post.author.name"></blog-post>
`` `

En los dos ejemplos anteriores, pasamos valores de cadena, pero cualquier tipo de valor se puede pasar a un prop.

### Pasando un número

`` `html
<!-- Even though `42` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.       -->
<blog-post v-bind:likes="42"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:likes="post.likes"></blog-post>
`` `

### Pasando un booleano

`` `html
<!-- Including the prop with no value will imply `true`. -->
<blog-post is-published></blog-post>

<!-- Even though `false` is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.          -->
<blog-post v-bind:is-published="false"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
`` `

### Pasando una matriz

`` `html
<!-- Even though the array is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.            -->
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
`` `

### Pasando un objeto

`` `html
<!-- Even though the object is static, we need v-bind to tell Vue that -->
<!-- this is a JavaScript expression rather than a string.             -->
<blog-post v-bind:author="{ name: 'Veronica', company: 'Veridian Dynamics' }"></blog-post>

<!-- Dynamically assign to the value of a variable. -->
<blog-post v-bind:author="post.author"></blog-post>
`` `

### Pasando las propiedades de un objeto

Si desea pasar todas las propiedades de un objeto como props, puede usar `v-bind` sin un argumento (` v-bind` en lugar de `v-bind: prop-name`) Por ejemplo, dado un objeto `post`:

`` `js
post: {
ID: 1,
Título: &#39;My Journey with Vue&#39;
}
`` `

La siguiente plantilla:

`` `html
<blog-post v-bind="post"></blog-post>
`` `

Será equivalente a:

`` `html
<blog-post
v-bind: id = &quot;post.id&quot;
v-bind: title = &quot;post.title&quot;
&gt; </blog-post>
`` `

## Flujo de datos unidireccional

Todos los accesorios forman un ** enlace unidireccional ** entre la propiedad secundaria y la principal: cuando la propiedad principal se actualice, fluirá hacia la secundaria, pero no al revés. Esto evita que los componentes secundarios muten accidentalmente el estado del padre, lo que puede hacer que los datos de su aplicación sean más difíciles de entender.

Además, cada vez que se actualiza el componente principal, todas las propiedades del componente secundario se actualizarán con el último valor. Esto significa que usted debe ** no ** intentar mutar un objeto dentro de un componente secundario. Si lo haces, Vue te avisará en la consola.

Normalmente hay dos casos en los que es tentador mutar un prop:

1. ** El prop es usado para pasar en un valor inicial; el componente hijo quiere usarlo como una propiedad de datos local más adelante. ** En este caso, es mejor definir una propiedad de datos local que use la propiedad como su valor inicial:

`` `js
accesorios: [&#39;initialCounter&#39;],
datos: function () {
regreso {
contador: this.initialCounter
}
}
`` `

2. ** El prop se pasa como un valor en bruto que debe transformarse. ** En este caso, es mejor definir una propiedad calculada utilizando el valor del prop:

`` `js
accesorios: [&#39;tamaño&#39;],
calculado: {
normalizedSize: function () {
devuelve this.size.trim (). toLowerCase ()
}
}
`` `

<p class="tip"> Tenga en cuenta que los objetos y las matrices en JavaScript se pasan por referencia, por lo que si la prop es una matriz u objeto, mutar el objeto o la matriz dentro del componente secundario ** afectará ** el estado principal. </p>

## Validación de Prop.

Los componentes pueden especificar requisitos para sus accesorios, como los tipos que ya ha visto. Si no se cumple un requisito, Vue le avisará en la consola de JavaScript del navegador. Esto es especialmente útil cuando se desarrolla un componente que está destinado a ser utilizado por otros.

Para especificar validaciones de prop, puede proporcionar un objeto con requisitos de validación al valor de `props`, en lugar de una matriz de cadenas. Por ejemplo:

`` `js
Vue.component (&#39;mi-componente&#39;, {
accesorios: {
// Comprobación de tipo básico (`null` coincide con cualquier tipo)
propA: Número,
// Múltiples tipos posibles
propB: [String, Number],
// Cadena requerida
Océano: {
tipo: cadena,
requerido: verdadero
}
// Número con un valor por defecto
propD: {
teclea un número,
por defecto: 100
}
// Objeto con un valor por defecto
por: {
tipo: objeto,
// Los valores predeterminados del objeto o matriz deben devolverse desde
// una función de fábrica
por defecto: function () {
devolver {mensaje: &#39;hola&#39;}
}
}
// función de validación personalizada
propF: {
validador: función (valor) {
// El valor debe coincidir con una de estas cadenas
return [&#39;success&#39;, &#39;warning&#39;, &#39;danger&#39;]. indexOf (value)! == -1
}
}
}
})
`` `

Cuando falla la validación de propiedades, Vue producirá una advertencia de consola (si se usa la versión de desarrollo).

<p class="tip"> Tenga en cuenta que las propiedades se validan ** antes ** se crea una instancia de componente, por lo que las propiedades de la instancia (por ejemplo, `data`,` computed`, etc.) no estarán disponibles dentro de las funciones `default` o` validator`. </p>

### Tipo de cheques

El `tipo` puede ser uno de los siguientes constructores nativos:

- Cuerda
- numero
- booleano
- Array
- objeto
- Fecha
- funcion
- simbolo

Además, `type` también puede ser una función constructora personalizada y la aserción se realizará con una comprobación` instanceof`. Por ejemplo, dada la siguiente función constructora existe:

`` `js
persona de la función (nombre, apellido) {
this.firstName = firstName
this.lastName = lastName
}
`` `

Podrías usar:

`` `js
View.component (&#39;blog-post&#39;, {
accesorios: {
autor: persona
}
})
`` `

para validar que el valor de la prop &#39;author` fue creado con `new Person`.

## Atributos no propuestos

Un atributo que no es de prop es un atributo que se pasa a un componente, pero no tiene definido un prop correspondiente.

Si bien los accesorios definidos explícitamente se prefieren para pasar información a un componente secundario, los autores de las bibliotecas de componentes no siempre pueden prever los contextos en los que se pueden usar sus componentes. Es por eso que los componentes pueden aceptar atributos arbitrarios, que se agregan al elemento raíz del componente.

Por ejemplo, imagine que estamos usando un componente `bootstrap-date-input` de terceros con un complemento Bootstrap que requiere un atributo` data-date-picker` en el `input`. Podemos agregar este atributo a nuestra instancia de componente:

`` `html
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
`` `

Y el atributo `data-date-picker =&quot; enabled &quot;` se agregará automáticamente al elemento raíz de `bootstrap-date-input`.

### Reemplazar / fusionar con atributos existentes

Imagina que esta es la plantilla para `bootstrap-date-input`:

`` `html
<input type="date" class="form-control">
`` `

Para especificar un tema para nuestro complemento de selector de fecha, podríamos necesitar agregar una clase específica, como esta:

`` `html
<bootstrap-date-input
data-date-picker = &quot;activado&quot;
class = &quot;date-picker-theme-dark&quot;
&gt; </bootstrap-date-input>
`` `

En este caso, se definen dos valores diferentes para `class`:

- `form-control`, que es establecido por el componente en su plantilla
- `date-picker-theme-dark`, que el padre pasa al componente

Para la mayoría de los atributos, el valor proporcionado al componente reemplazará el valor establecido por el componente. Entonces, por ejemplo, pasar `type =&quot; text &quot;` reemplazará a `type =&quot; date &quot;` y probablemente lo rompa! Afortunadamente, los atributos `class` y` style` son un poco más inteligentes, por lo que ambos valores se combinan, haciendo que el valor final sea: `form-control date-picker-theme-dark`.

### Deshabilitando la herencia de atributos

Si ** no ** quiere que el elemento raíz de un componente herede los atributos, puede establecer `inheritAttrs: false` en las opciones del componente. Por ejemplo:

`` `js
Vue.component (&#39;mi-componente&#39;, {
inheritAttrs: false,
// ...
})
`` `

Esto puede ser especialmente útil en combinación con la propiedad de instancia `$ attrs`, que contiene los nombres de atributos y valores pasados a un componente, como:

`` `js
{
clase: &#39;entrada de usuario&#39;,
marcador de posición: &#39;Ingrese su nombre de usuario&#39;
}
`` `

Con `inheritAttrs: false` y` $ attrs`, puede decidir manualmente a qué elemento desea enviar los atributos, lo que a menudo es deseable para [componentes base] (../ style-guide / # Base-component-names-strong -recomendado):

`` `js
Vue.component (&#39;entrada base&#39;, {
inheritAttrs: false,
accesorios: [&#39;etiqueta&#39;, &#39;valor&#39;],
plantilla: `
<label>
{{etiqueta}}
<input
v-bind = &quot;$ attrs&quot;
v-bind: valor = &quot;valor&quot;
v-on: input = &quot;$ emit (&#39;input&#39;, $ event.target.value)&quot;
&gt;
</label>
`
})
`` `

Este patrón le permite usar componentes base más como elementos HTML sin procesar, sin tener que preocuparse por qué elemento está realmente en su raíz:

`` `html
<base-input
v-model = &quot;nombre de usuario&quot;
class = &quot;nombre de usuario-entrada&quot;
placeholder = &quot;Ingrese su nombre de usuario&quot;
&gt; </base-input>
`` `

