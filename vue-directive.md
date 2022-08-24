[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Directives structurelles

## v-if v-else

````html
<script>
import { ref } from 'vue'

setup() {
	const awesome = ref(true)

	function toggle() { awesome.value = !awesome.value }
	retur { awesome, toggle }
}
</script>

<template>
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</template>
````
[Back to top](#directives-structurelles)     
  
## v-for

````html
<script>
import { ref } from 'vue'

setup() {
	// give each todo a unique id
	let id = 0

	const newTodo = ref('')
	const todos = ref([
	  { id: id++, text: 'Learn HTML' },
	  { id: id++, text: 'Learn JavaScript' },
	  { id: id++, text: 'Learn Vue' }
	])

	function addTodo() {
	  todos.value.push({ id: id++, text: newTodo.value})
	  // reset todo
	  newTodo.value = ''
	}

	function removeTodo(todo) {
	  todos.value = todos.value.filter(t => t.id !== todo.id)
	}
	return { addTodo, newTodo, todos, removeTodos }
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>    
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>
````
[Back to top](#directives-structurelles)     
