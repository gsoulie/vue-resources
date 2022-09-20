[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Référence computed


Les fonctions computed permettent de retourner une valeur calculée. Par exemple nous recevons une liste de restaurant avec une image, et nous souhaitons
calculer la chaîne css qui permettra de positionner le background-image correspondant à chaque restaurant.

Il est possible d'utiliser une fonction "classique" et l'utiliser dans la vue sans aucun souci mis à part qu'à chaque modification des données, vue va
générer un nouveau rendu et donc rejouer la fonction. 
Les fonction **computed** en revanche, ont l'avantage d'être plus performantes dans le cas d'un calcul d'info dans la vue car elles font du **caching**. 
On pourrait aussi comparer les fonction *computed* aux **PipeTransform** d'Angular.

Comment le cache des fonctions computed ?

1 : Vue va chercher des données réactives dans votre fonction computed     
2 : La première exécution de la fonction computed va créer un cache, qu’il trouve ou non une donnée réactive     
3 : Si au prochain rendu (à l’update), il voit qu’une donnée réactive utilisée dans le computed a changé, il ré-exécute le computed pour créer un nouveau résultat et le remet en cache     
4 : S’il ne trouve aucune dépendance (donnée réactive), il renverra toujours le même résultat, celui du cache précédent.     

> IMPORTANT : Vérifier s'il est possible de passer des paramètres à une computed !!!

*RestaurantCard.vue*

````html
<template>
  <div class="restaurant-card">
    <div :style="changeBackground" class="restaurant-img"></div>
    {{ restaurant.name }}
  </div>
</template>

<script lang="ts">
import Restaurant from '@/models/restaurant'
import { computed } from 'vue'	// <--- import de computed

export default {
    name: 'RestaurantCard',
    props: {
        restaurant: Restaurant
    },
    setup(props) {
        const changeBackground = computed(() => {	// <-- Fonction computed
            return {
                backgroundImage: `url(${props.restaurant?.image})`.toString()
            }
        })

		// IMPORTANT :  retourner cette fonction à la fin du setup
        return {
            changeBackground
        }
    }
}
</script>
````
	
### exemple pipe qui double une valeur

*Angular*
````typescript
@Component({
	selector: 'app-doublecount',
	template: '<div>{{ number | double }}</div>',
})
export class DoublecountComponent {
	count: number = 10;
}
````
	
*Vue*
````html
<script>
import { ref, computed } from 'vue';
	
setup() {
	const count = ref(10);
	const doubleCount = computed(() => count.value * 2);
	return { count, doubleCount }
}
</script>

<template>
	<div>{{ doubleCount }}</div>
</template>
````

### Autre exemple
````html
<script>
import { ref, computed } from 'vue'

setup() {
	let id = 0

	const newTodo = ref('')
	const hideCompleted = ref(false)
	const todos = ref([
	  { id: id++, text: 'Learn HTML', done: true },
	  { id: id++, text: 'Learn JavaScript', done: true },
	  { id: id++, text: 'Learn Vue', done: false }
	])
	const filteredTodos = computed(() => {
	  // return filtered todos based on
	  // `todos.value` & `hideCompleted.value`
	  return hideCompleted.value
		? todos.value.filter((t) => !t.done)
		: todos.value
	})

	function addTodo() {
	  todos.value.push({ id: id++, text: newTodo.value, done: false })
	  newTodo.value = ''
	}

	function removeTodo(todo) {
	  todos.value = todos.value.filter((t) => t !== todo)
	}
	return { newTodo, todos, hideCompleted, addTodo, removeTodo, filteredTodos}
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" />
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
      
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done { text-decoration: line-through; }
</style>
````
