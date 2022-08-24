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

> Il est impératif de déclarer une *:key* pour garantir l'ordre interne des données dans le composant.
  
## v-for

````html
<template>
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>
	
<!-- Declaring Index as key -->
	
<ul>
  <li v-for="(index, user) in users" :key="index">
    {{ user.name }}
  </li>
</ul>
</template>
````
[Back to top](#directives-structurelles)     
