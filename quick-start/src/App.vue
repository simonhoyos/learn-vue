<script setup lang="ts">
import * as Vue from 'vue'
import ChildComponent from './ChildComponent.vue'

const counter = Vue.reactive({
  count: 0
})

const message = Vue.ref('Hello Vue 3')
const titleClass = Vue.ref('title')

const count = Vue.ref(0)

function increment() {
  counter.count++
  count.value++
}

const text = Vue.ref('')

// function onInput(event) {
//   text.value = event.target.value
// }

let id = 0

const isShown = Vue.ref(true)

setTimeout(() => {
  isShown.value = false
}, 1000)

const todos = Vue.ref([
  { id: id++, text: 'Learn Vue 3', done: true },
  { id: id++, text: 'Learn React', done: false },
  { id: id++, text: 'Learn Angular', done: false }
])

const hideCompleted = Vue.ref(false)

const filteredTodos = Vue.computed(() => {
  const filtered =
    hideCompleted.value !== true ? todos.value : todos.value.filter((todo) => todo.done !== true)
  console.log('filtered', filtered)
  return filtered
})

console.log(counter.count)
console.log(message.value)
console.log(text.value)

const newTodo = Vue.ref('')

function addTodo() {
  console.log('creating todo', newTodo.value)
  todos.value.push({
    id: id++,
    text: newTodo.value,
    done: false
  })
  newTodo.value = ''
}

function removeTodo(id: number) {
  console.log('removing ', id)
  todos.value = todos.value.filter((todo) => todo.id !== id)
}

Vue.watch(count, (newValue, oldValue) => {
  console.log('count changed', newValue, oldValue)
})

const elementRef = Vue.ref<HTMLParagraphElement | null>(null)

Vue.onMounted(() => {
  if (elementRef.value == null) return
  elementRef.value.textContent = 'hello world'
})

Vue.onUpdated(() => {
  if (elementRef.value == null) return
  elementRef.value.textContent = 'my ref updated'
})

// const message = Vue.ref('Hello world')
</script>

<template>
  <h1 v-bind:class="titleClass">{{ message }}</h1>
  <p>reactive {{ counter.count }}</p>
  <p>ref {{ count }}</p>
  <button @click="increment">Increment</button>

  <input v-model="text" />
  <!-- <input :value="text" @input="onInput" /> -->

  <p v-if="isShown">If</p>
  <p v-else>Else</p>

  <form @submit.prevent="addTodo">
    <input v-model="newTodo" placeholder="new todo" required />
    <button>Add todo</button>
  </form>

  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <!-- <input type="checkbox" :checked="todo.done" /> -->
      <input type="checkbox" :class="{ done: todo.done }" v-model="todo.done" />
      <span>{{ todo.text }}</span>

      <button type="button" @click="removeTodo(todo.id)">X</button>
    </li>
  </ul>

  <button type="button" @click="hideCompleted = !hideCompleted">Hide completed</button>

  <p ref="elementRef">My ref</p>

  <ChildComponent :message="message" @response="(msg) => (message = msg)">
    <div>slot content</div>
  </ChildComponent>
</template>

<style>
.title {
  color: red;
}
.done {
  text-decoration: line-through;
}
</style>
