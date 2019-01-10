---
title: Unit Testing
type: guide
order: 403
---

## Instalación y herramientas

Cualquier cosa compatibl con un sistema de construcción basado en módulos funcionará, pero si usted esta buscando una recomendación específica puede [Karma](http://karma-runner.github.io). Este tiene una comunidad con muchos plugins, incluyendo soporte para  [Webpack](https://github.com/webpack/karma-webpack) y [Browserify](https://github.com/Nikku/karma-browserify). Para una instalación detallada por favor vea la documentación respectiva a cada proyecto. Este es un ejemplo de configuración de Karma para [Webpack](https://github.com/vuejs-templates/webpack/blob/master/template/test/unit/karma.conf.js) y [Browserify](https://github.com/vuejs-templates/browserify/blob/master/template/karma.conf.js) que puede ayudarle a comenzar.

## Asersiones simples

Usted no tiene que hacer nada especial en sus componentes para hacerlos testeables. ExporteYou don't have to do anything special in your components to make them testable. Exporte las opciones tal cual:

``` html
<template>
  <span>{{ message }}</span>
</template>

<script>
  export default {
    data () {
      return {
        message: 'hello!'
      }
    },
    created () {
      this.message = 'bye!'
    }
  }
</script>
```

Entonces importe las opciones del componente junto con Vue, y usted prodrá hacer muchas asersiones comunes:

``` js
// Import Vue and the component being tested
import Vue from 'vue'
import MyComponent from 'path/to/MyComponent.vue'

// Here are some Jasmine 2.0 tests, though you can
// use any test runner / assertion library combo you prefer
describe('MyComponent', () => {
  // Inspect the raw component options
  it('has a created hook', () => {
    expect(typeof MyComponent.created).toBe('function')
  })

  // Evaluate the results of functions in
  // the raw component options
  it('sets the correct default data', () => {
    expect(typeof MyComponent.data).toBe('function')
    const defaultData = MyComponent.data()
    expect(defaultData.message).toBe('hello!')
  })

  // Inspect the component instance on mount
  it('correctly sets the message when created', () => {
    const vm = new Vue(MyComponent).$mount()
    expect(vm.message).toBe('bye!')
  })

  // Mount an instance and inspect the render output
  it('renders the correct message', () => {
    const Constructor = Vue.extend(MyComponent)
    const vm = new Constructor().$mount()
    expect(vm.$el.textContent).toBe('bye!')
  })
})
```

## Escribiendo Componentes Testeables

El resultado de la renderización de un componente es determinado principalmente por las propiedades que recibe. Si el resultado de la renderización de un componente depende unicamente de sus accesorios, se vuelve sencillo de probar, similar a asertar el valor de retorno de una función pura con diferentes argumentos. Tomemos un ejemplo simplificado:

``` html
<template>
  <p>{{ msg }}</p>
</template>

<script>
  export default {
    props: ['msg']
  }
</script>
```

Usted puede afirmar el resultado del renderizado con diferentes props usando la opción `propsData`:

``` js
import Vue from 'vue'
import MyComponent from './MyComponent.vue'

// helper function that mounts and returns the rendered text
function getRenderedText (Component, propsData) {
  const Constructor = Vue.extend(Component)
  const vm = new Constructor({ propsData: propsData }).$mount()
  return vm.$el.textContent
}

describe('MyComponent', () => {
  it('renders correctly with different props', () => {
    expect(getRenderedText(MyComponent, {
      msg: 'Hello'
    })).toBe('Hello')

    expect(getRenderedText(MyComponent, {
      msg: 'Bye'
    })).toBe('Bye')
  })
})
```

## Asertaciones de Aactualizaciones Asincrónicas

Como Vue [realiza actualizaciones asincrónicas del DOM](reactivity.html#Async-Update-Queue), las afirmaciones sobre las actualizaciones de DOM resultantes del cambio de estado deberán realizarse en una devolución de llamada `Vue.nextTick`:

``` js
// Inspect the generated HTML after a state update
it('updates the rendered message when vm.message updates', done => {
  const vm = new Vue(MyComponent).$mount()
  vm.message = 'foo'

  // wait a "tick" after state change before asserting DOM updates
  Vue.nextTick(() => {
    expect(vm.$el.textContent).toBe('foo')
    done()
  })
})
```

Planeamos trabajar en una coleccion de helpers comunes para testing para hacer esto más simple en el renderizado de componentes con diferentes restricciones (ejemplo. una representación superficial que ignora componentes secundarios) y asertar su resultado.

Para información más detallada sobre testing unitario en Vue, visite [vue-test-utils](https://vue-test-utils.vuejs.org/) y nuestra publicación de cookbook [testing unitario con componentes Vue](https://vuejs.org/v2/cookbook/unit-testing-vue-components.html).
