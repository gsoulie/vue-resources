[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Bases

* [Introduction](#introduction)      
* [Commandes](#commandes)      
* [Extensions VSCode](#extensions-vscode)     
* [Frameworks UI](#frameworks-ui)     
* [Lifecycle hook](#lifecycle-hook)      
* [Raccourcis](#raccourcis)     
* [Event listener](#event-listener)     
* [Directives structurelles](#directives-structurelles)      
* [2-way binding](#2--way-binding)      
* [Reference computed](#reference-computed)     
* [Watchers](#watchers)       
* [Composants](#composants)       
* [props](#props)     
* [Fonction computed](#fonction-computed)     
* [Slot template content](#slot-template-content)     
* [Routing](#routing)     
* [Divers](#divers)     


## Introduction

Le code présenté ici utilise l'api *composition* et la syntaxe *SFC (Single File Component)* ainsi que l'utilisation du script ````<script setup>````

La fonctionnalité principale de Vue est le **rendu déclaratif** : en utilisant une syntaxe de modèle qui étend le HTML, nous pouvons décrire à quoi le HTML devrait ressembler en fonction de l'état de JavaScript. Lorsque l'état change, le HTML se met à jour automatiquement

L'état qui peut déclencher des mises à jour lorsqu'il est modifié est considéré comme **reactive**. Nous pouvons déclarer l'état réactif en utilisant l'API ````reactive()```` de Vue. Les objets créés à partir de ````reactive()```` sont des proxies JavaScript qui fonctionnent comme des objets normaux

````reactive()*```` ne fonctionne que sur les objets (y compris les tableaux et les types intégrés comme Map et Set). ````ref()````, d'autre part, peut prendre n'importe quel type de valeur et créer un objet qui expose la valeur interne sous une propriété ````.value````

Les variables qui ne sont pas utilisées dans la vue n'ont pas besoin d'être déclarée comme ````reactive()```` ou ````ref()````.

## Commandes
| Directive        | Raccourcis           |
| ------------- |:-------------:|
|````npm i -g @vue/cli````|installer le CLI globalement|
|````npm u -g @vue/cli````|mettre à jour le CLI|
|````vue create <your_project>````|créer un nouveau projet|
|````vue ui````|utiliser l'interface graphique pour créer un projet|
|````vue serve````|exécuter un serve|
|````vue-cli-service build --mode production````|compiler en mode production (voir plus bas)|
|````npm run serve````|exécuter un serve avec hot reload|

**CONSEIL** utiliser ````vue ui```` pour pouvoir configurer manuellement toutes les options (typescript, sass, router, vuex...)

*package.json*
````json
...
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build --mode production"	// <-- rajouter le mode de build. commande dans le terminal : $vue build
  },
  ...
````

*Obtenir un projet complètement configuré avec routing etc...*

````
npm init vue@latest
cd <project_directory>
npm i
npm  run dev
````

## Extensions VSCode

* vetur (snippet)    
* vue 3 snippets highlight formatters     
  
[Back to top](#bases)   

## Frameworks UI

https://athemes.com/collections/vue-ui-component-libraries/      

**ATTENTION** tous les frameworks ne supportent pas encore Vue3

Ok avec Vue 3 :
* https://quasar.dev/     
* https://www.primefaces.org/primevue/setup     
* https://buefy.org/documentation/start     
* https://demos.creative-tim.com/vue-material-kit/documentation/#getting-started      
* https://element-plus.org/en-US/guide/installation.html     

### PrimeVue

````
npm install primevue@^3.12.6 --save
npm install primeicons --save
````

**Importer un thème **

Prime propose une multitude de thème (accessibles depuis le bouton engrenage du side menu https://www.primefaces.org/primevue/setup)

*Exemple import thème par défaut* (imports à ajouter dans le *main.ts*

````
import 'primevue/resources/themes/saga-blue/theme.css'       //theme
import 'primevue/resources/primevue.min.css'                 //core css
import 'primeicons/primeicons.css'                           //icons
````

**Configuration main.ts*

*main.ts*
````typescript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

import PrimeVue from 'primevue/config'		// importer la config de base PrimeView
import Button from 'primevue/button'		// importer les composants boutons
import InputText from 'primevue/inputtext'	// importer les composants inputs

// importer les thèmes
import 'primevue/resources/themes/saga-blue/theme.css'       //theme
import 'primevue/resources/primevue.min.css'                 //core css
import 'primeicons/primeicons.css'                           //icons


//createApp(App).use(store).use(router).mount('#app')	// à remplacer par :

const app = createApp(App);
app.use(PrimeVue);

// déclarer les composants utilisés
app.component('InputText', InputText);	
app.component('Button', Button);

app.mount('#app');

````

## Lifecycle hook

https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram      

* onMounted = ngOnInit()      
* onUpdated      
* onUnmounted = ngOnDestroy()     

So far, Vue has been handling all the DOM updates for us, thanks to reactivity and declarative rendering. However, inevitably there will be cases where we need to manually work with the DOM.

We can request a template ref - i.e. a reference to an element in the template - using the special ref attribute:

Notice the ref is initialized with null value. This is because the element doesn't exist yet when <script setup> is executed. The template ref is only accessible after the component is mounted.

````html
<script setup>
import { ref, onMounted } from 'vue'

const p = ref(null)

onMounted(() => { p.value.textContent = 'mounted!' })
</script>

<template>
  <p ref="p">hello</p>
</template>
````
  
## Raccourcis

| Directive        | Raccourcis           |
| ------------- |:-------------:|
|````v-bind:<value>````|````:<value>````|
|````v-bind:class````|````:class````|
|````v-bind:id````|````:id````|
|````v-on:click````|````@click````|
|````v-on:<event_source>````|````@<event_source>````|
|````v-on:input````|````@input````|
  
[Back to top](#bases)   
  
  
## Event listener
  
````html
<script setup>
import { ref } from 'vue'

const count = ref(0)
let count2 = 0

function increment() {
  count2++
  count.value++
}
</script>

<template>
  <!-- make this button work -->
  <button @click="increment">count is: {{ count }}</button>
  <p>
    Count2 : {{ count2 }}
  </p>
</template>
````
[Back to top](#bases)   
  
  
## Directives structurelles
  
### v-if v-else

````html
<script setup>
import { ref } from 'vue'

const awesome = ref(true)

function toggle() { awesome.value = !awesome.value }
</script>

<template>
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</template>
````
[Back to top](#bases)     
  
### v-for

````html
<script setup>
import { ref } from 'vue'

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
[Back to top](#bases)   
  
  
## 2-way binding

Le 2-way binding avec vue se caractérise par l'utilisation du v-bind + v-on. Pour simplifier cela on peut simplement utiliser ````v-model="maVar"````
  
````html
<script setup>
import { ref } from 'vue'

const text = ref('')

function onInput(e) { text.value = e.target.value }
</script>

<template>
  <input :value="text" @input="onInput" placeholder="Type here">
  
  <!-- Or to simplify -->
  <input v-model="text" placeholder="Type here">
  
  <p>{{ text }}</p>
</template>
````
[Back to top](#bases)   
  
  
## Reference computed
 
> équivalent aux pipeTransform Angular
	
*exemple pipe qui double une valeur*

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
<script setup>
import { ref, computed } from 'vue';
const count = ref(10);
const doubleCount = computed(() => count.value * 2);
</script>

<template>
	<div>{{ doubleCount }}</div>
</template>
````

**Autre exemple**
````html
<script setup>
import { ref, computed } from 'vue'

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
.done {
  text-decoration: line-through;
}
</style>
````
[Back to top](#bases)   
  
## Watchers

Les watchers permettent de déclencher un traitement lorsque la valeur d'un élément observé change. 

https://vuejs.org/guide/essentials/watchers.html

**Ex 1 : déclencher un affichage console lorsqu'une valeur est modifiée**
  
````html
<script setup>
import { ref, watch } from 'vue'

const count = ref(0)

function increment() { count.value++ }
 
// watcher
watch(count, (newCount) => {
  console.log(`new count is ${newCount}`)
})
  
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

**Ex 2 : Lister des todos à chaque incrémentation du todoId**
  
````html
<script setup>
import { ref, watch } from 'vue'

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
</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>
````
[Back to top](#bases)     
  
## Composants
  
````html
<script setup>
	import ChildComp from './ChildComp.vue'
</script>

<template>
  <ChildComp></ChildComp>
</template>
````
  
### Passage de paramètres

> équivalent @Input() Angular 

Le composant enfant doit définir la liste des paramètres qu'il expose

````html
<script setup>
const props = defineProps({
  msg: String,
  count: Number
})
</script>
````

Ces propriétés sont accessibles depuis le code via l'objet retourné par defineProps() et sont accessible aux parents de la manière suivante via un v-bind

````html
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const greeting = ref('Hello from parent')

</script>

<template>
  <ChildComp :msg="greeting" />
</template>
````
  
### Events
  
> équivalent @Output() Angular

Déclaration des évènements depuis le composant enfant

````html
<script setup>
// declare emitted events
const emit = defineEmits(['response'])

// emit with argument
emit('response', 'hello from child')
</script>
````
  
Réaction à l'évènement depuis le parent

````html
<ChildComp @response="(msg) => childMsg = msg" />
````

**Ex d'écoute :**

````html
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const childMsg = ref('No child msg yet')
</script>

<template>
  <ChildComp @:response="(msg) => childMsg = msg"/>
  <p>{{ childMsg }}</p>
</template>  
````  

````html
<script setup>
import { ref, reactive } from 'vue'

  let fixedMessage = 'Hello World !'
  const dynamicMessage = ref('My name is ')	// Déclaré avec ref(...) l'objet expose une propriété value
  dynamicMessage.value += 'toto';
// component logic
const counter = reactive({
  count: 1
})
</script>

<template>
  <h1>{{ fixedMessage }}</h1>
  <h4>
    {{ dynamicMessage }}
  </h4>
  <p>
    {{counter.count}}
  </p>
</template>

<style>
  h1 {
    color: coral;
  }
</style>
````
	
[Back to top](#bases)    

## props

Les **props** sont comparables aux ````@Input()```` d'Angular. Elles permettent de partager des variables avec d'autres composants

### Exposer les props

Pour pouvoir exposer une variable en tant que *props* il faut au préalable la retourner à la fin du *setup* via la fonction ````return````.

Ensuite dans la vue on peut passer cette variable à d'autres composants en déclarant la propriété de la manière suivante ````<RestaurantRow :restaurants="data"></RestaurantRow>````

*home.vue*

````html
<template>
  <div class="home">
    <RestaurantRow :restaurants="data"></RestaurantRow>
  </div>
</template>

<script lang="ts">

import { ref, onMounted } from 'vue';

export default {
    name: 'Home',
    components: {
        RestaurantRow
    },
    setup() {
        const data = ref<Restaurant[]>([]); // ref() nécessaire pour déclarer la variable "observable"
        // sans le ref(), toute modification des données ne déclencherai pas de changement dans la vue

        /**
         * ie : ngOnInit
         */
        onMounted(() => {
            initializeDatabase();
        });
        
        function initializeDatabase() {
            // remplir data    
			...
        }  
        
        // Dernière étape du Setup()
        return {
            data,   // indiquer qu'on souhaite exposer data aux autres composants
        }
    }
}
</script>
````

Côté composant "enfant", pour accéder à la props, il faut déclarer la props que l'on reçoit à la manière du ````@Input()```` d'Angular 

*RestaurantRow.vue* 

````html
<template>
	<div class="row">
		<RestaurantCard 
		class="cell"
		v-for="(r, index) in restaurants" :key="index" :restaurant="r"></RestaurantCard>
	</div>
</template>

<script lang="ts">
import RestaurantCard from './RestaurantCard.vue'

export default {
    name: 'RestaurantRow',
    components: {
        RestaurantCard
    },
    props: {
        restaurants: {
		type: Array,	// <--- déclaration de la props (ici c'est un tableau de Restaurant)
		required: true
	}
	// !! Ou alors, si option required non nécessaire :
	restaurants: Array,
    } 	
}
</script>
```` 

### Accéder aux props dans le setup

````html
<script lang="ts">
import Restaurant from '@/models/restaurant'
export default {
    name: 'RestaurantCard',
    props: {
        restaurant: Restaurant
    },
	setup(props) {	// <--- permet d'accéder aux props dans le setup
	
	}
}
</script>
````

[Back to top](#bases)   
## Fonction computed

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
	
[Back to top](#bases)   
  
## Slot template content
  
> équivalent *ng-content* Angular

Il suffit au parent de passer du contenu html dans la balise du contenu enfant

*composant parent*
````html
<ChildComp>
  <p>
    This is the contant template from the parent {{ msg }}
  </p>
</ChildComp>
````

Côté composant enfant, il est possible de définir un contenu "par défaut" lorsque le parent ne passe aucun contenu via la balise slot 

*composant enfant*
````html
<slot>Fallback content</slot>
````
[Back to top](#bases)   
  
## Routing
	
https://router.vuejs.org/guide/essentials/navigation.html#navigate-to-a-different-location

*router/index.ts*

````typescript
import { createRouter, createWebHistory } from "vue-router";
import HomeView from "../views/HomeView.vue";

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: "/",
      name: "home",
      component: HomeView,
    },
    {
      path: "/about",
      name: "about",
      // route level code-splitting
      // this generates a separate chunk (About.[hash].js) for this route
      // which is lazy-loaded when the route is visited.
      component: () => import("../views/AboutView.vue"),
    },
  ],
});

export default router;
````

*component.vue*

````html
<template>
  <nav>
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/about">About</RouterLink>
  </nav>
</template>
	
<script setup lang="ts">
	import router from '@/router';
	
	//routing par code
	// literal string path
	router.push('/users/eduardo')

	// object with path
	router.push({ path: '/users/eduardo' })

	// named route with params to let the router build the url
	router.push({ name: 'user', params: { username: 'eduardo' } })

	// with query, resulting in /register?plan=private
	router.push({ path: '/register', query: { plan: 'private' } })

	// with hash, resulting in /about#team
	router.push({ path: '/about', hash: '#team' })
</script>
````
[Back to top](#bases)       

## Divers

ajouter une classe / id css dynamique

````html
<script setup>
	import { ref } from 'vue'

	const titleClass = ref('title')
	const myDivId = ref('myDiv')
</script>

<template>
  <div :id="myDivId">
  	<h1 :class="titleClass">Make me red</h1> <!-- add dynamic class binding here -->
  </div>
</template>

<style>
	.title { color: red; }
	#myDivId { 
	  padding: 20px;
	  background: coral;
	}
</style>
````
[Back to top](#bases)     
