[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Watchers

* [Généralités et exemples](#généralités-et-exemples)     
* [propriété deep](#propriété-deep)     

## Généralités et exemples

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
	
        // Observer les changements de la variable "name"
        watch(name, (newVal: string) => {
            localStorage.setItem('name', newVal);	// mémorisation de la valeur en local storage à chaque modification de cette dernière
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

## Propriété deep

<img src="https://img.shields.io/badge/Important-DD0031.svg?logo=LOGO"> Pour l'observation des objets et tableaux il est **nécessaire** d'ajouter la propriété ````deep: true```` au watcher.

En effet, de base, les watchers n'observent que la modification de la valeur observée. Ils sont donc déclenchés en cas de réaffectation d'une valeur. Dans le cas des objets et tableaux, l'ajout, suppresssion d'un élément n'est pas détecté. 

Il faut donc indiquer au watcher qu'on souhaite observer la moindre modification (modification, ajout et suppression).

````
// Observer les changements de la liste todos
watch(todos, (newVal: ITodo[]) => {
	localStorage.setItem('todos', JSON.stringify(newVal));
}, { deep: true });
````
