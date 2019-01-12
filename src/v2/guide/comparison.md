---
Título: Comparación con otros marcos
tipo: guía
orden: 801
---

Esta es definitivamente la página más difícil de escribir en la guía, pero sentimos que es importante. Lo más probable es que haya tenido problemas que haya intentado resolver y que haya utilizado otra biblioteca para resolverlos. Estás aquí porque quieres saber si Vue puede resolver mejor tus problemas específicos. Eso es lo que esperamos responder por ti.

También nos esforzamos mucho por evitar sesgos. Como equipo central, obviamente nos gusta mucho Vue. Hay algunos problemas que creemos que se resuelven mejor que cualquier otra cosa. Si no lo creyéramos, no estaríamos trabajando en ello. Sin embargo, queremos ser justos y precisos. Cuando otras bibliotecas ofrecen ventajas significativas, como el vasto ecosistema de renderizadores alternativos de React o el soporte del navegador de Knockout para IE6, también intentamos enumerarlos.

¡También nos gustaría ** su ** ayuda para mantener este documento actualizado porque el mundo de JavaScript se mueve rápidamente! Si observa una imprecisión o algo que no parece correcto, háganoslo saber [abriendo un problema] (https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+ comparaciones + guía).

## reaccionar

React y Vue comparten muchas similitudes. Ambos:

- utilice la virtual DOM
- Proporcionar componentes de vista reactivos y compostables.
- mantener el enfoque en la biblioteca central, con preocupaciones como el enrutamiento y la administración global del estado que manejan las bibliotecas complementarias

Al ser tan similares en su alcance, hemos dedicado más tiempo a ajustar esta comparación que a cualquier otro. Queremos garantizar no solo la precisión técnica, sino también el equilibrio. Señalamos donde React supera a Vue, por ejemplo, en la riqueza de su ecosistema y la abundancia de sus renderizadores personalizados.

Dicho esto, es inevitable que la comparación aparezca sesgada hacia Vue para algunos usuarios de React, ya que muchos de los temas explorados son, hasta cierto punto, subjetivos. Reconocemos la existencia de diferentes gustos técnicos, y esta comparación apunta principalmente a delinear las razones por las cuales Vue podría ser una mejor opción si sus preferencias coinciden con las nuestras.

Algunas de las secciones a continuación también pueden estar ligeramente desactualizadas debido a las actualizaciones recientes en React 16+, y estamos planeando trabajar con la comunidad React para renovar esta sección en un futuro próximo.

### Rendimiento en tiempo de ejecución

Tanto React como Vue son excepcionalmente rápidos y similares, por lo que es poco probable que la velocidad sea un factor decisivo para elegir entre ellos. Sin embargo, para métricas específicas, consulte este [punto de referencia de terceros] (http://www.stefankrause.net/js-frameworks-benchmark7/table.html), que se centra en el rendimiento de procesamiento / actualización sin formato con árboles de componentes muy simples.

#### esfuerzos de optimización

En Reaccionar, cuando cambia el estado de un componente, activa la representación de todo el subárbol de componentes, comenzando en ese componente como raíz. Para evitar repeticiones innecesarias de componentes secundarios, debe usar `PureComponent` o implementar` shouldComponentUpdate` siempre que pueda. También es posible que deba usar estructuras de datos inmutables para que los cambios de estado sean más fáciles de optimizar. Sin embargo, en ciertos casos es posible que no pueda confiar en tales optimizaciones porque &#39;PureComponent / shouldComponentUpdate&#39; asume que la salida de procesamiento de todo el subárbol está determinada por las propiedades del componente actual. Si ese no es el caso, entonces tales optimizaciones pueden llevar a un estado DOM inconsistente.

En Vue, las dependencias de un componente se rastrean automáticamente durante su procesamiento, por lo que el sistema sabe con precisión qué componentes necesitan realmente volver a renderizarse cuando cambia el estado. Se puede considerar que cada componente tiene implementado automáticamente `shouldComponentUpdate`, sin las advertencias del componente anidado.

En general, esto elimina la necesidad de una clase completa de optimizaciones de rendimiento de la placa del desarrollador, y les permite centrarse más en la construcción de la aplicación a medida que escala.

### HTML y CSS

En React, todo es solo JavaScript. Las estructuras HTML no solo se expresan a través de JSX, sino que las tendencias recientes también tienden a colocar la administración de CSS dentro de JavaScript. Este enfoque tiene sus propios beneficios, pero también viene con varias concesiones que pueden no valer la pena para cada desarrollador.

Vue adopta las tecnologías web clásicas y se basa en ellas. Para mostrarte lo que eso significa, veremos algunos ejemplos.

#### JSX vs Plantillas

En React, todos los componentes expresan su IU dentro de las funciones de renderizado utilizando JSX, una sintaxis declarativa tipo XML que funciona dentro de JavaScript.

Las funciones de render con JSX tienen algunas ventajas:

- Puede aprovechar el poder de un lenguaje de programación completo (JavaScript) para construir su vista. Esto incluye variables temporales, controles de flujo y referenciar directamente los valores de JavaScript en el alcance.

- El soporte de herramientas (p. Ej., Alineación, comprobación de tipos, autocompletado del editor) para JSX es en algunos aspectos más avanzado que lo que está disponible actualmente para las plantillas Vue.

En Vue, también tenemos [funciones de procesamiento] (render-function.html) e incluso [soporte JSX] (render-function.html # JSX), porque a veces sí necesitas esa potencia. Sin embargo, como experiencia predeterminada, ofrecemos plantillas como una alternativa más simple. Cualquier HTML válido también es una plantilla de Vue válida, y esto conlleva algunas ventajas propias:

- Para muchos desarrolladores que han estado trabajando con HTML, las plantillas se sienten más naturales para leer y escribir. La preferencia en sí misma puede ser algo subjetiva, pero si hace que el desarrollador sea más productivo, el beneficio es objetivo.

- Las plantillas basadas en HTML facilitan la migración progresiva de las aplicaciones existentes para aprovechar las características de reactividad de Vue.

- También hace que sea mucho más fácil para los diseñadores y los desarrolladores menos experimentados analizar y contribuir al código base.

- Incluso puedes usar preprocesadores como Pug (anteriormente conocido como Jade) para crear tus plantillas de Vue.

Algunos argumentan que necesitarías aprender un DSL adicional (lenguaje específico del dominio) para poder escribir plantillas; creemos que esta diferencia es superficial en el mejor de los casos. Primero, JSX no significa que el usuario no necesite aprender nada, es una sintaxis adicional en la parte superior de JavaScript simple, por lo que puede ser fácil de aprender para alguien familiarizado con JavaScript, pero decir que es esencialmente gratuito es engañoso. De manera similar, una plantilla es solo una sintaxis adicional sobre HTML simple y, por lo tanto, tiene un costo de aprendizaje muy bajo para aquellos que ya están familiarizados con HTML. Con el DSL también podemos ayudar al usuario a hacer más con menos código (por ejemplo, modificadores `v-on`). La misma tarea puede implicar mucho más código cuando se usan funciones JSX o de renderización simples.

En un nivel superior, podemos dividir los componentes en dos categorías: los de presentación y los de lógica. Recomendamos el uso de plantillas para los componentes de presentación y la función de representación / JSX para las lógicas. El porcentaje de estos componentes depende del tipo de aplicación que está creando, pero en general encontramos que las presentaciones son mucho más comunes.

#### Componente CSS

A menos que distribuya componentes en varios archivos (por ejemplo con [Módulos CSS] (https://github.com/gajus/react-css-modules)), el alcance del CSS en React se realiza a menudo a través de soluciones CSS-in-JS por ejemplo, [componentes de estilo] (https://github.com/styled-components/styled-components), [glamour] (https://github.com/paypal/glamorous), y [emoción] (https: // github.com/emotion-js/emotion)). Esto introduce un nuevo paradigma de estilo orientado a componentes que es diferente del proceso de creación de CSS normal. Además, aunque hay soporte para extraer CSS en una sola hoja de estilo en el momento de la compilación, todavía es común que se deba incluir un tiempo de ejecución en el paquete para que el estilo funcione correctamente. Mientras obtiene acceso al dinamismo de JavaScript mientras construye sus estilos, la compensación es a menudo un aumento en el tamaño del paquete y el costo del tiempo de ejecución.

Si eres un fanático de CSS-in-JS, muchas de las bibliotecas populares de CSS-in-JS admiten Vue (por ejemplo, [styled-components-vue] (https://github.com/styled-components/vue-styled- componentes) y [vue-emotion] (https://github.com/egoist/vue-emotion)). La principal diferencia entre React y Vue aquí es que el método predeterminado de estilo en Vue es a través de etiquetas &#39;style` más familiares en [componentes de un solo archivo] (single-file-components.html).

[Componentes de un solo archivo] (single-file-components.html) le brindan acceso completo a CSS en el mismo archivo que el resto del código del componente.

`` `html
<style scoped>
@media (ancho mínimo: 250 px) {
.list-container: hover {
fondo: naranja;
}
}
</style>
`` `

El atributo opcional `scoped` ajusta automáticamente este CSS a su componente agregando un atributo único (como` data-v-21e5b78`) a los elementos y compilando `.list-container: hover` a algo como` .list-container [ data-v-21e5b78]: hover`.

Por último, el estilo de los componentes de un solo archivo de Vue es muy flexible. A través de [vue-loader] (https://github.com/vuejs/vue-loader), puede usar cualquier preprocesador, postprocesador e incluso una profunda integración con [CSS Modules] (https: // vue-loader vuejs.org/en/features/css-modules.html) - todo dentro del ` <style>` element.

### Escala

#### Ampliar

Para aplicaciones grandes, tanto Vue como React ofrecen soluciones de enrutamiento robustas. La comunidad React también ha sido muy innovadora en términos de soluciones de administración estatal (por ejemplo, Flux / Redux). Estos patrones de administración de estado e [incluso el propio Redux] (https://yarnpkg.com/en/packages?q=redux%20vue&amp;p=1) se pueden integrar fácilmente en las aplicaciones de Vue. De hecho, Vue incluso ha llevado este modelo un paso más allá con [Vuex] (https://github.com/vuejs/vuex), una solución de administración de estado inspirada en Elm que se integra profundamente en Vue que creemos que ofrece una experiencia de desarrollo superior. .

Otra diferencia importante entre estas ofertas es que las bibliotecas complementarias de Vue para la administración del estado y el enrutamiento (entre [otras inquietudes] (https://github.com/vuejs)) se admiten oficialmente y se mantienen actualizadas con la biblioteca central. En su lugar, React elige dejar estas preocupaciones a la comunidad, creando un ecosistema más fragmentado. Sin embargo, al ser más popular, el ecosistema de React es considerablemente más rico que el de Vue.

Finalmente, Vue ofrece un [generador de proyectos de CLI] (https://github.com/vuejs/vue-cli) que hace que sea trivialmente fácil comenzar un nuevo proyecto utilizando su sistema de compilación, incluyendo [webpack] (https: / /github.com/vuejs-templates/webpack), [Browserify] (https://github.com/vuejs-templates/browserify), o incluso [no build system] (https://github.com/vuejs-templates /sencillo). React también está avanzando en esta área con [create-react-app] (https://github.com/facebookincubator/create-react-app), pero actualmente tiene algunas limitaciones:

- No permite ninguna configuración durante la generación del proyecto, mientras que las plantillas de proyecto de Vue permiten la personalización como [Yeoman] (http://yeoman.io/).
- Solo ofrece una única plantilla que asume que está creando una aplicación de una sola página, mientras que Vue ofrece una amplia variedad de plantillas para diversos propósitos y sistemas de compilación.
- No puede generar proyectos a partir de plantillas creadas por el usuario, lo que puede ser especialmente útil para entornos empresariales con convenciones preestablecidas.

Es importante tener en cuenta que muchas de estas limitaciones son decisiones de diseño intencional tomadas por el equipo de la aplicación crear-reaccionar y tienen sus ventajas. Por ejemplo, siempre que las necesidades de su proyecto sean muy simples y nunca necesite &quot;expulsar&quot; para personalizar su proceso de compilación, podrá actualizarlo como una dependencia. Puede leer más sobre [filosofía diferente] aquí (https://github.com/facebookincubator/create-react-app#philosophy).

#### Descendiendo

React es famoso por su empinada curva de aprendizaje. Antes de que pueda comenzar, necesita saber acerca de JSX y probablemente de ES2015 +, ya que muchos ejemplos utilizan la sintaxis de clase de React. También debe aprender acerca de los sistemas de compilación, ya que aunque técnicamente podría usar Babel Standalone para compilar en vivo su código en el navegador, no es absolutamente adecuado para la producción.

Si bien Vue se amplía tan bien como React, también lo hace tan bien como jQuery. Así es: para comenzar, todo lo que tiene que hacer es colocar una sola etiqueta de script en la página:

`` `html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
`` `

Luego, puede comenzar a escribir el código Vue e incluso enviar la versión reducida a producción sin sentirse culpable o tener que preocuparse por los problemas de rendimiento.

Como no necesita saber acerca de JSX, ES2015 o los sistemas de compilación para comenzar a utilizar Vue, por lo general, a los desarrolladores les lleva menos de un día leer [la guía] (./) para aprender lo suficiente como para crear aplicaciones no triviales.

### Representación nativa

React Native le permite escribir aplicaciones con renderizado nativo para iOS y Android usando el mismo modelo de componente React. Esto es excelente ya que, como desarrollador, puede aplicar su conocimiento de un marco en múltiples plataformas. En este frente, Vue tiene una colaboración oficial con [Weex] (https://weex.apache.org/), un marco de interfaz de usuario multiplataforma creado por Alibaba Group e incubado por Apache Software Foundation (ASF). Weex le permite usar la misma sintaxis de componentes de Vue para crear componentes que no solo se pueden representar en el navegador, sino también de forma nativa en iOS y Android.

En este momento, Weex aún se encuentra en desarrollo activo y no está tan maduro ni probado como React Native, pero su desarrollo está impulsado por las necesidades de producción del negocio de comercio electrónico más grande del mundo, y el equipo de Vue también participará activamente. colabore con el equipo de Weex para garantizar una experiencia fluida para los desarrolladores de Vue.

Otra opción que los desarrolladores de Vue pronto tendrán es [NativeScript] (https://www.nativescript.org/), a través de un [complemento impulsado por la comunidad] (https://github.com/rigor789/nativescript-vue).

### Con MobX

MobX se ha vuelto bastante popular en la comunidad React y en realidad usa un sistema de reactividad casi idéntico a Vue. Hasta cierto punto, el flujo de trabajo React + MobX se puede considerar como un Vue más detallado, por lo que si usa esa combinación y la disfruta, saltar a Vue es probablemente el siguiente paso lógico.

### Preact y otras bibliotecas similares a reaccionar

Las bibliotecas similares a React generalmente intentan compartir la mayor parte de su API y ecosistema con React como sea posible. Por esa razón, la gran mayoría de las comparaciones anteriores también se aplicarán a ellos. La principal diferencia será típicamente un ecosistema reducido, a menudo significativamente, en comparación con React. Dado que estas bibliotecas no pueden ser 100% compatibles con todo lo que se encuentra en el ecosistema React, es posible que algunas bibliotecas de herramientas y complementarias no sean utilizables. O, incluso si parecen funcionar, podrían romperse en cualquier momento a menos que su biblioteca específica React-like sea oficialmente compatible a la par con React.

## AngularJS (Angular 1)

Parte de la sintaxis de Vue se verá muy similar a AngularJS (por ejemplo, `v-if` vs` ng-if`). Esto se debe a que AngularJS acertó muchas cosas y estas fueron una inspiración para Vue muy temprano en su desarrollo. También hay muchos dolores que vienen con AngularJS, donde Vue ha intentado ofrecer una mejora significativa.

### Complejidad

Vue es mucho más simple que AngularJS, tanto en términos de API como de diseño. Aprender lo suficiente para crear aplicaciones no triviales generalmente toma menos de un día, lo que no es cierto para AngularJS.

### Flexibilidad y modularidad

AngularJS tiene opiniones sólidas sobre cómo deberían estructurarse sus aplicaciones, mientras que Vue es una solución modular más flexible. Si bien esto hace que Vue sea más adaptable a una amplia variedad de proyectos, también reconocemos que a veces es útil que se tomen algunas decisiones por usted, para que pueda comenzar a codificar.

Es por eso que ofrecemos una [plantilla de paquete web] (https://github.com/vuejs-templates/webpack) que puede configurarlo en cuestión de minutos, al mismo tiempo que le otorga acceso a funciones avanzadas como la recarga de módulos en caliente, el forro, la extracción CSS , y mucho más.

### El enlace de datos

AngularJS utiliza enlaces bidireccionales entre ámbitos, mientras que Vue aplica un flujo de datos unidireccional entre componentes. Esto hace que el flujo de datos sea más fácil de razonar en aplicaciones no triviales.

### Directivas vs Componentes

Vue tiene una separación más clara entre directivas y componentes. Las directivas están destinadas a encapsular las manipulaciones de DOM solamente, mientras que los componentes son unidades autónomas que tienen su propia lógica de vista y datos. En AngularJS, las directivas hacen todo y los componentes son solo un tipo específico de directiva.

### Rendimiento en tiempo de ejecución

Vue tiene un mejor rendimiento y es mucho más fácil de optimizar porque no utiliza la comprobación sucia. AngularJS se vuelve lento cuando hay muchos observadores, porque cada vez que cambia algo en el alcance, todos estos observadores deben ser reevaluados nuevamente. Además, el ciclo de resumen puede tener que ejecutarse varias veces para &quot;estabilizarse&quot; si algún observador desencadena otra actualización. Los usuarios de AngularJS a menudo tienen que recurrir a técnicas esotéricas para sortear el ciclo de resumen, y en algunas situaciones, no hay forma de optimizar un alcance con muchos observadores.

Vue no sufre de esto en absoluto porque utiliza un sistema transparente de observación de seguimiento de dependencias con colas asíncronas: todos los cambios se activan de forma independiente a menos que tengan relaciones de dependencia explícitas.

Curiosamente, hay algunas similitudes en cómo Angular y Vue están abordando estos problemas de AngularJS.

## Angular (Anteriormente conocido como Angular 2)

Tenemos una sección separada para el nuevo Angular porque realmente es un marco completamente diferente al de AngularJS. Por ejemplo, cuenta con un sistema de componentes de primera clase, muchos detalles de la implementación se han reescrito por completo y la API también ha cambiado de manera drástica.

### TypeScript

Angular esencialmente requiere el uso de TypeScript, dado que casi toda su documentación y recursos de aprendizaje están basados en TypeScript. TypeScript tiene sus beneficios: la comprobación de tipos estática puede ser muy útil para aplicaciones a gran escala y puede ser un gran impulso para la productividad de los desarrolladores con fondos en Java y C #.

Sin embargo, no todo el mundo quiere usar TypeScript. En muchos casos de uso a pequeña escala, la introducción de un sistema tipo puede generar más gastos generales que el aumento de la productividad. En esos casos, sería mejor ir con Vue, ya que usar Angular sin TypeScript puede ser un desafío.

Finalmente, aunque no está tan profundamente integrado con TypeScript como Angular, Vue también ofrece [tipificaciones oficiales] (https://github.com/vuejs/vue/tree/dev/types) y [decorador oficial] (https: // github .com / vuejs / vue-class-component) para aquellos que desean usar TypeScript con Vue. También estamos colaborando activamente con los equipos de TypeScript y VSCode en Microsoft para mejorar la experiencia de TS / IDE para los usuarios de Vue + TS.

### Rendimiento en tiempo de ejecución

Ambos marcos son excepcionalmente rápidos, con métricas muy similares en los puntos de referencia. Puede [navegar por métricas específicas] (http://www.stefankrause.net/js-frameworks-benchmark7/table.html) para una comparación más granular, pero es poco probable que la velocidad sea un factor decisivo.

### Tamaño

Versiones recientes de Angular, con [compilación AOT] (https://en.wikipedia.org/wiki/Ahead-of-time_compilation) y [sacudida de árboles] (https://en.wikipedia.org/wiki/Tree_shaking) , han podido bajar su tamaño considerablemente. Sin embargo, un proyecto Vue 2 con todas las funciones con Vuex + Vue Router incluido (~ 30KB gzipped) aún es significativamente más ligero que una aplicación compilada por AOT y lista para usar, generada por `angular-cli` (~ 65KB gzipped) .

### flexibilidad

Vue tiene una opinión mucho menor que Angular, ya que ofrece soporte oficial para una variedad de sistemas de compilación, sin restricciones en la forma en que estructura su aplicación. Muchos desarrolladores disfrutan de esta libertad, mientras que otros prefieren tener una sola forma correcta de crear cualquier aplicación.

### Curva de aprendizaje

Para comenzar con Vue, todo lo que necesita es familiaridad con HTML y ES5 JavaScript (es decir, JavaScript simple). Con estas habilidades básicas, puede comenzar a crear aplicaciones no triviales en menos de un día de lectura [la guía] (./).

La curva de aprendizaje de Angular es mucho más pronunciada. La superficie API del marco es enorme y como usuario necesitará familiarizarse con muchos más conceptos antes de ser productivo. La complejidad de Angular se debe en gran medida a su objetivo de diseño de apuntar solo a aplicaciones grandes y complejas, pero eso hace que el marco sea mucho más difícil para los desarrolladores menos experimentados.

## hombre

Ember es un marco con todas las funciones que está diseñado para ser altamente valorado. Proporciona muchas convenciones establecidas y una vez que esté lo suficientemente familiarizado con ellas, puede hacer que sea muy productivo. Sin embargo, también significa que la curva de aprendizaje es alta y la flexibilidad sufre. Es una compensación cuando se trata de elegir entre un marco de opinión y una biblioteca con un conjunto de herramientas acopladas de manera flexible que funcionan en conjunto. Este último te da más libertad, pero también requiere que tomes más decisiones de arquitectura.

Dicho esto, probablemente haría una mejor comparación entre Vue core y Ember [templating] (https://guides.emberjs.com/v2.10.0/templates/handlebars-basics/) y [object model] (https: // guides.emberjs.com/v2.10.0/object-model/) layers:

- Vue proporciona una reactividad discreta en objetos JavaScript simples y propiedades computadas totalmente automáticas. En Ember, debe envolver todo en Objetos Ember y declarar manualmente las dependencias para las propiedades computadas.

- La sintaxis de la plantilla de Vue aprovecha toda la potencia de las expresiones de JavaScript, mientras que la expresión y la sintaxis del ayudante de Handlebars es intencionalmente bastante limitada en comparación.

- En cuanto al rendimiento, Vue supera a Ember [por un margen justo] (http://www.stefankrause.net/js-frameworks-benchmark7/table.html), incluso después de la última actualización del motor Glimmer en Ember 2.x. Vue procesa automáticamente las actualizaciones por lotes, mientras que en Ember necesita administrar manualmente los bucles de ejecución en situaciones críticas de rendimiento.

## Knockear

Knockout fue un pionero en los espacios de seguimiento de dependencias y MVVM y su sistema de reactividad es muy similar al de Vue. Su [compatibilidad con el navegador] (http://knockoutjs.com/documentation/browser-support.html) también es muy impresionante, teniendo en cuenta todo lo que hace, ¡con soporte para IE6! Por otro lado, Vue solo es compatible con IE9 +.

Sin embargo, con el tiempo, el desarrollo de Knockout se ha ralentizado y ha comenzado a mostrar un poco su edad. Por ejemplo, su sistema de componentes carece de un conjunto completo de ganchos de ciclo de vida y, si bien es un caso de uso muy común, la interfaz para pasar hijos a un componente se siente un poco torpe en comparación con [Vue&#39;s] (components.html # Content-Distribution-with- Ranuras).

También parece haber diferencias filosóficas en el diseño de la API que, si usted es curioso, puede demostrarse cómo cada uno maneja la creación de una [lista de tareas simple] (https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89). Definitivamente es algo subjetivo, pero muchos consideran que la API de Vue es menos compleja y está mejor estructurada.

## Polímero

Polymer es otro proyecto patrocinado por Google y, de hecho, también fue una fuente de inspiración para Vue. Los componentes de Vue pueden compararse libremente con los elementos personalizados de Polymer y ambos proporcionan un estilo de desarrollo muy similar. La mayor diferencia es que Polymer se basa en las características más recientes de componentes web y requiere que los polifills no triviales funcionen (con un rendimiento degradado) en los navegadores que no admiten esas características de forma nativa. Por el contrario, Vue funciona sin dependencias ni rellenos automáticos hasta IE9.

En Polymer, el equipo también ha hecho que su sistema de enlace de datos sea muy limitado para compensar el rendimiento. Por ejemplo, las únicas expresiones compatibles con las plantillas de Polymer son la negación booleana y las llamadas a un solo método. Su implementación de la propiedad computada tampoco es muy flexible.

## Alboroto

Riot 3.0 proporciona un modelo de desarrollo basado en componentes similar (que se llama &quot;etiqueta&quot; en Riot), con una API mínima y bellamente diseñada. Riot y Vue probablemente comparten mucho en las filosofías de diseño. Sin embargo, a pesar de ser un poco más pesado que Riot, Vue ofrece algunas ventajas significativas:

- Mejor presentación. Riot [atraviesa un árbol DOM] (http://riotjs.com/compare/#virtual-dom-vs-expressions-binding) en lugar de usar un DOM virtual, por lo que sufre los mismos problemas de rendimiento que AngularJS.
- Soporte de herramientas más maduro. Vue proporciona soporte oficial para [webpack] (https://github.com/vuejs/vue-loader) y [Browserify] (https://github.com/vuejs/vueify), mientras que Riot confía en el apoyo de la comunidad para el sistema de compilación integración.

