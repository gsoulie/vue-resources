[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Watchers

Les *watchers* permettent de déclencher un traitement dès lors qu'une modification intervient sur la valeur observée.

https://vuejs.org/guide/essentials/watchers.html      

````html
<template>
	<input 
	v-model="searchField"
	type="text">
</template>

<script lang="ts">
import { ref, watch } from 'vue';

export default {
    name: 'Header',
    setup() {
        let searchField = ref('');
        
        watch(searchField, (newValue, oldValue) => {
            console.log(newValue);
        });

        return {
            searchField	// exposer searchField à la vue
        }
    }
}
</script>
````

### Ex 1 : déclencher un affichage console lorsqu'une valeur est modifiée
  
````html
<script>
import { ref, watch } from 'vue'

setup() {
	const count = ref(0)

	function increment() { count.value++ }

	// watcher
	watch(count, (newCount) => {
	  console.log(`new count is ${newCount}`)
	})
	return { count, increment }
}  
</script>

<template>
  <button @click="increment">
    Increment
  </button>
  <p>
    {{ count }}
  </p>
</template>
````

### Ex 2 : Lister des todos à chaque incrémentation du todoId
  
````html
<script>
import { ref, watch } from 'vue'

setup() {
	const todoId = ref(1)
	const todoData = ref(null)

	async function fetchData() {
	  todoData.value = null
	  // timeout pour simuler le chargement
	  setTimeout(async () => {
	    const res = await fetch(
	      `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
	    )  
	    todoData.value = await res.json()
	  }, 500)  

	}
	// watcher qui déclenche le chargement du todo lors de l'incrémentation du todoId
	watch(todoId, fetchData)  

	fetchData()
	return { todoId, todoData }
}
</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>
````
