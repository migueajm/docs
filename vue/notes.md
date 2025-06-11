# Notes
### Configuración de la aplicación
```javascript

/**
 * **config** object
 * handle error
 */
app.config.errorHandler = (error) => {
	/* hande error */
}

/**
 * **component** object
 * component register
 */
app.component('componentName', componentName)
```

### Multiple instances
```js
const app = createApp({/* root component options */});
const app2 = createApp({});
app.mount('#app');
app.mount('#app2');
```
#### En general, v-iftiene costos de alternancia más altos, al igual que v-showcostos de renderizado inicial más altos. Por lo tanto, es preferible v-showsi necesita alternar algo con frecuencia y v-ifsi es improbable que la condición cambie durante la ejecución.

### Directivas
```md
- v-html: Procesar html
- v-bind: Para definir atributos dinamicos en los elementos html(Si el valor enlazado es nullo undefined, el atributo se eliminará del elemento renderizado. <div v-bind:id="dynamicId"></div>);Tambien tiene una sintaxis abreviada (<div :id="dynamicId"></div>); Se puede simplificar aun más si la variable tiene el mismo nombre(<div :id></div> o <div v-bind:id></div>)
- v-if: debe estar asociada a un solo elemento. Pero ¿qué pasa si queremos alternar más de un elemento? En este caso, podemos usar " v-if" en un <template>, este actua como un contenedor.
- v-else-if
- v-else: esta directiva debe ir despues de un "v-if" o "v-else-if" de lo contrario no sera reconocida.
- v-on: Escucha eventos del DOM (<a v-on:click="doSomething"> ... </a> o <a @click="doSomething"> ... </a>)
- v-show: similar a "v-if" pero el elemento siempre se procesa independientemente de la condición inicial, con alternancia basada en CSS.
- v-for: Si se usa en conjunto con "v-if" este siempre se evaluara primero(No se recomienda usar v-if"y" v-foren el mismo elemento debido a la precedencia implícita) <li v-for="item in items"> o <li v-for="(item, index) in items"> o <div v-for="item of items"></div>, tambien se puede usar para iterar propiedades de un objeto <li v-for="value in myObject"> o <li v-for="(value, key) in myObject"> o <li v-for="(value, key, index) in myObject">
- v-model: sincroniza el valor de un input con el esstado de una variable
```

#### No se puede usar la interpolación dentro de los atributos HTML, para ello esta la directiva **v-bind**

### Sintaxis de plantilla
```js
// Interpolación(interpretan los datos como texto plano, no como HTML)
<span>Message: {{ msg }}</span>

//Procesar HTML
<p>Using v-html directive: <span v-html="rawHtml"></span></p>

// Enlaces de atributos
<div v-bind:id="dynamicId"></div>
<div :id="dynamicId"></div>
<div :id="dynamicId"></div>
<div :id></div>
<div v-bind:id></div>

// Vinculación dinámica de multiples atributos
data() {
  return {
    objectOfAttrs: {
      id: 'container',
      class: 'wrapper'
    }
  }
}
<div v-bind="objectOfAttrs"></div>

// Expresiones JS
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```
### nextTick
```js
// **nextTick** si necesitas hacer algo después de que el DOM se haya actualizado, usas await nextTick().
import { ref, nextTick } from 'vue'

const show = ref(false)

function openModal() {
  show.value = true

  // Aquí el DOM aún no se ha actualizado, el modal todavía no está presente
  await nextTick()

  // Ahora puedes acceder al modal en el DOM, hacer focus, scroll, etc.
  const modal = document.querySelector('#myModal')
  modal?.focus()
}
```
###  Computed Caching vs. Methods
```js
// Computed Caching vs. Methods: En cuanto al resultado final, ambos enfoques son idénticos.
Sin embargo, la diferencia radica en que las propiedades calculadas se almacenan en caché según sus dependencias reactivas.
Una propiedad calculada solo se reevaluará cuando algunas de sus dependencias reactivas hayan cambiado.
Esto significa que, siempre que author.booksno haya cambiado, el acceso múltiple a publishedBooksMessagedevolverá
inmediatamente el resultado calculado previamente sin tener que volver a ejecutar la función getter.

computed: {
	// a computed getter
	publishedBooksMessage() {
		// `this` points to the component instance
		return this.author.books.length > 0 ? 'Yes' : 'No'
	}
}
<span>{{ publishedBooksMessage }}</span>

methods: {
  calculateBooksMessage() {
    return this.author.books.length > 0 ? 'Yes' : 'No'
  }
}
<p>{{ calculateBooksMessage() }}</p>
```
### Modificadores
```js
// Modificadores de eventos
.stop
.prevent
.self
.capture
.once
.passive
<form @submit.prevent="onSubmit">...</form>
// Modificadores de tecla
.enter
.tab
.delete(captura las teclas "Eliminar" y "Retroceso")
.esc
.space
.up
.down
.left
.right
// Teclas modificadoras del sistema
.ctrl
.alt
.shift
.meta // windows, command
<!-- only call `submit` when the `key` is `Enter` -->
<input @keyup.enter="submit" />
<!-- Alt + Enter -->
<input @keyup.alt.enter="clear" />

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
// Modificadores de mouse
.left
.right
.middle
.exact //Modificador​
El .exact modificador permite controlar la combinación exacta de modificadores del sistema necesarios para activar un evento.

// Modificadores de inputs
.lazy
<!-- synced after "change" instead of "input" -->
<input v-model.lazy="msg" />
.number
.trim
```

### watchers

```js
// metodo "watch" funciona directamente con "ref"
/* 
  El primer argumento puede ser distintos tipos de "fuentes" reactivas:
  puede ser una referencia (incluidas las referencias calculadas),
  un objeto reactivo, una función getter o una matriz de múltiples fuentes:
*/
const x = ref(0)
const y = ref(0)

// single ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})
// getter
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// array of multiple sources
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})

const obj = reactive({ count: 0 })

// this won't work because we are passing a number to watch()
watch(obj.count, (count) => {
  console.log(`Count is: ${count}`)
})

// instead, use a getter:
watch(
  () => obj.count,
  (count) => {
    console.log(`Count is: ${count}`)
  }
)
// Deep Watchers
//Sin embargo, puedes forzar el segundo caso en un observador profundo usando explícitamente la "deep" opción:

watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // Note: `newValue` will be equal to `oldValue` here
    // *unless* state.someObject has been replaced
  },
  { deep: true }
)

// Eager Watchers

watch(
  source,
  (newValue, oldValue) => {
    // executed immediately, then again when `source` changes
  },
  { immediate: true }
)

//once watchers compatible con vue 3.4 +
watch(
  source,
  (newValue, oldValue) => {
    // when `source` changes, triggers only once
  },
  { once: true }
)

// watchEffect
const todoId = ref(1)
const data = ref(null)

watch(
  todoId,
  async () => {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
    )
    data.value = await response.json()
  },
  { immediate: true }
)

watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```

### Template refs
```js
// Aatributo especial, similar al "key" de la directiva "v-for"
<input ref="input">
// acceso a las referencias
<script setup>
import { useTemplateRef, onMounted } from 'vue'

// the first argument must match the ref value in the template
const input = useTemplateRef('my-input')

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="my-input" />
</template>
```
