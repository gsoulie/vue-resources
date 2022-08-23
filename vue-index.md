# Ressources Vue3

* [Bases](https://github.com/gsoulie/vue-resources/blob/main/vue-basics.md)      
* [Présentation](#presentation)      
* [>> Projet exemple](https://github.com/gsoulie/vue-example-ubereats)      
* [Initialisation projet](#initialisation-projet)       
* [Events](#emit-events)     
* [Watcher](#watcher)     
* [Routing, guard, titre](#routing)     
* [Composable](#composable)      
* [Appels api avec Axios](#appels-api-avec-axios)       
* [Vue state avec vuex](#vue-state-avec-vuex)     


## Présentation

https://www.jesuisundev.com/comprendre-vuejs-en-5-minutes/      

VueJS est un framework Javascript frontend pour créer des interfaces utilisateurs. Tu vas me dire « encore un ? » et la réponse est oui. Sauf qu’il est un peu différent.

Déjà, c’est intéressant de comprendre que VueJS a été conçu pour être intégré de façon incrémentale. Ça veut dire que **si tu as une application frontend déjà existante, t’as pas à tout refaire. Tu peux faire une nouvelle partie en VueJS et l’intégrer rapidement au reste**.

VueJS prend aussi les bonnes idées de ses concurrents. Il permet le data binding. Les données et le DOM sont couplés et réactifs aux changements. On retrouve également le concept de virtual dom avec VueJS. Le DOM n’est pas directement changé, ça passe par le virtual DOM.

On retrouve aussi l’organisation par composant. Cette fonctionnalité permet de découper ton application en plusieurs sous-composants qui gèrent chacun leur vie et qui sont réutilisables. Imaginons tu veux faire une liste d’images : tu peux faire un composant qui gère l’image et un composant qui gère une liste de composant image.

VueJS se concentre sur la partie vue de ton application. Pour ce faire, le framework s’inspire en partie du patron d’architecture MVVM. VueJS va lier ton DOM, la partie vue, avec ton instance de vue, la partie Vue-Modèle. Ces deux parties sont liées par le système de data-binding

[Back to top](#ressources-vue3)     

## Initialisation projet

### styles

Créer un fichier *style.scss* dans un répertoire *src/styles* pour initialiser les styles de bases et centraliser tous les styles génériques

*styles.scss*
````css
html {
	line-height: 1.15; /* 1 */
	-webkit-text-size-adjust: 100%; /* 2 */
}  
body {
	margin: 0;
}  
main {
	display: block;
}
a {
	text-decoration: none;
}
````

Import dans le *App.vue*
````scss
<style lang="scss">
@import url('./styles/style.scss');
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400&display=swap');
#app {
  font-family: 'Roboto', Sans-serif;
  -webkit-font-smoothing: antialiased;	// important
  -moz-osx-font-smoothing: grayscale;	// important
  text-align: center;
  color: #2c3e50;  
}
body {
  padding: 0px;
}
</style>
````

[Back to top](#ressources-vue3)     

## Emit events

https://www.youtube.com/watch?v=EEeaG0BTBQo&ab_channel=LearnVue

### Solution 1 : émettre depuis la vue : $emit

*Parent.vue*
````html
<template>
	<Header @response="searchRestaurant"></Header>
</template>

<script>
...
setup() {
	searchRestaurant(event) {
		console.log(event);
	}
}
</script>
````

*Child.vue*
````html
<template>
<input 
    v-model="searchField" 
    @change="$emit('response', $event.target.value)"
    type="text" 
    placeholder="Que chezchez vous ?"/>
</template>
````


### Solution 2 - émettre depuis le script avec un custom event

*Child.vue*
````html
<template>
<input 
    v-model="searchField" 
    @change="sendChange"
    type="text" 
    placeholder="Que chezchez vous ?"/>
</template>

<script lang="ts">
import { ref, watch } from 'vue';

export default {
    name: 'Header',
    emits: ["customChange"],	// <--- déclarer le nom de l'event sinon warning
    setup(props, { emit }) {	// <--- utilisation directe de la fonction emit avec destructuration du context.emit
        
		const sendChange = (event) => {
			emit("customChange", event.target.value)	// <--- utilisation directe du emit
		}
		
        return {
            sendChange
        }
    }
}
</script>
````

**Autre syntaxe simplifiée possible**

````typescript
<script lang="ts">
import { ref, watch } from 'vue';

export default {
    name: 'Header',
    // emits: ["customChange"], // <--- non nécessaire
    setup(props, context ) {	// <--- syntaxe sans destructuration, 'context' peut prendre le nom que l'on souhaite
        const sendChange = (event) => {
		context.emit("customChange", event.target.value)	// <--- appeler <nom>.emit
	}
	...
    }
}
</script>
````



[Back to top](#ressources-vue3)     

## watcher

Les *watchers* permettent de déclencher un traitement dès lors qu'une modification intervient sur la valeur observée.

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
[Back to top](#ressources-vue3)     

## Routing

### Passage de paramètres en mode props

*route.ts*

````typescript
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    redirect: 'home'
  },
  {
    path: '/home',
    name: 'home',
    component: () => import('../page/Home.vue')
  },
  {
    path: '/restaurant/:restaurantName',
    name: 'restaurant',
    props: true,	// <--- permet la récupération du paramètre comme une props
    component: () => import('../page/Restaurant.vue')
  }
]
````

*Parent.vue*

````html
<template>
	<button @click="navigateToItem">Open</button>
</template>

<script lang="ts">
import router from '@/router';

export default {
    name: 'Parent',
    setup() {
        function navigateToItem() {
            router.push({ name: 'restaurant', params: { restaurantName: 'Subway' }});
        }

        return {
            navigateToItem
        }
    }
}
</script>
````

*Restaurant.vue*

````html
<template>
  <div class="content">
    <h1>{{ restaurantName }}</h1>
	
	<!-- ##### AUTRE SOLUTION ##### -->
	<h1>{{ $route.params.restaurantName }}</h1>
  </div>
</template>

<script lang="ts">

export default {
    name: 'RestaurantModale',
    props: [
        'restaurantName'	// <--- paramètre de routing reçu en tant que props
    ]
}
</script>

<style>

</style>
````
[Back to top](#ressources-vue3)     

### Lister les params d'une route

````typescript
import router from '@/router';

setup() {
	const params = router.currentRoute.value.params
	console.log('params', params);
}
````
[Back to top](#ressources-vue3)     

### Routing dans la vue

````html
<template>
    <router-link 
        class="restaurant-card" 
        :to="{ name: 'restaurant', params: { restaurantName: restaurant.name }}"
        style="text-decoration: none; color: inherit;">
        
        <div class="content">
          <!-- some content here -->
        </div>
    </router-link>
</template>
````
[Back to top](#ressources-vue3)     

### Modifier le titre des pages

*router/index.ts*

````typescript
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'home',
    component: HomeView,
    meta: {
      title: 'Home'
    }
  },
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

// Changer le titre des pages
router.beforeEach((to, from, next) => {
  document.title = `${to.meta.title} | Active Tracker`
});
````
[Back to top](#ressources-vue3)     

### Route guard

*router/index.ts*
````typescript
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'home',
    component: HomeView,
    meta: {
      title: 'Home',
	  auth: false	// <--
    }
  },{
    path: '/login',
    name: 'login',
    component: () => import('../views/Login.vue'),
    meta: {
      title: 'Login',
      auth: false	// <--
    }
  },{
    path: '/create',
    name: 'create',
    component: () => import('../views/Create.vue'),
    meta: {
      title: 'Create',
      auth: true	// <--
    }
  },
  { 
    path: '/:pathMatch(.*)*', 	// <-- Page 404
    name: 'not-found', 
    component: () => import('../views/Error404.vue'),
  }
]

// Guard
router.beforeEach((to, from, next) => {
  // Vérifier la condition voulue : ici on vérifie que l'utilisateur est authentifié avec supabase
  const user = supabase.auth.user();  // récupérer l'état d'authentification de l'utilisateur supabase

  if (to.matched.some((res) => res.meta.auth)) {
    if (user) {
      next();
      return;
    }
    next({name: 'login'});	// <-- redirection vers le login
    return;
  }
  next();	// <-- laisser continuer la redirection
});
````

[Back to top](#ressources-vue3)     

## Composable

Un code partagé entre plusieurs composant peut être extrait dans un troisième composant qui sera appelé dans chacun des autres composants ayant besoin de ce même code. Avec Vue, une autre solution existe, les **Composables**. Les Composables permettent de partager du code *controller* à la manière d'un *serivce* Angular

Exemple de composable

*composables/mouse.ts*

````typescript
import {ref, onMounted, onUnmounted } from 'vue';

export function useMouse() {
	const x = ref(0);
	const y = ref(0);
	
	function update(event) {
		x.value = event.pageX;
		y.value = event.pageY;
	}
	
	onMounted(() => window.addEventListener('mousemove', update));
	onUnmounted(() => window.addEventListener('mousemove', update));
	
	return { x, y }
}
````

Utilisation du composable

*Home.vue*
````html
<template>
	Mosue position at : {{ x }}, {{ y }}
</template>
<script>
import { useMouse } from './composables/mouse';
setup() {
	const { x, y } = useMouse();
}
</script>
````
[Back to top](#ressources-vue3)     

## Appels api avec Axios

**Axios** est une librairie permettant de réaliser des appels http

````npm i axios````

*Home.vue*
````typescript
<script>
	import axios from 'axios
	export default {
		setup() {
			data = ref([])
			
			onMounted(async () => {	// <--- ne pas oublier le async
				fetchData();
			})
			
			async function fetchData() {
				try {
					const { data } = await axios.get('https://dummyjson.com/products');
            				console.log(data);
				} catch(e) {
				
				}
			}
			
			const createPost = () => {
				axios.post('https://dummyjson.com/products/add',
					JSON.stringify(
						id: 99,
						title: 'akdapdokzap',
						...
					)
				)
				.then(response => {
					console.log(response);
				})
				.catch(e => console.warn(e));
			}
		}
	}
</script>
````

````axios.get()```` retourne une promise
[Back to top](#ressources-vue3)     

### Bonne pratique - ApiHelper

Créer un service ApiHelper

*apiHelper.ts*

````typescript
import axios from 'axios'

export default(url='http://api.kanye.rest') => {	//<-- url par défaut
	return axios.create({
		baseURL: url
	})
}
````

Ensuite créer autant de services que nécessaires pour chaque domaines

*KanyeAPI.ts*
````typescript
import apiHelper from './apiHelper'

export default {
	getQuote() {
		return apiHelper().get('/')
	}
	
	createPost(data) {
		return apiHelper('https://jsonplaceholder.typicode.com/').post('/posts', data)
	}
}
````

Appel depuis un composant

*Home.vue*

````typescript
import KanyeAPI from './services/KanyeAPI'

<script>
	export default {
		setup() {
			const quote = ref('');
			
			const loadQuote = async () => {
				const response = await KanyeAPI.getQuote()
				quote.value = response.data.quote
			}
			
			onMounted(async () => { loadQuote() })
			
			const createPost = async () => {
				const response = KanyeAPI.createPost(JSON.stringify({
					title: 'foo',
					body: 'bar',
					userId: 1
				}))
				.catch(e => console.warn(e))
				
				console.log(response)
			}
		}
	}
</script>
````
[Back to top](#ressources-vue3)     

https://www.youtube.com/watch?v=fh0VboqAc8k&ab_channel=ProgramWithErik       


## Vue state avec vuex

**vuex** permet de gérer le state de l'application et de répercuter ses modifications automatiquement.

* State = les variables     
* Getter = accesseurs des variables du state     
* Mutations = modificateurs **synchrones** des variables du state     
* Actions = modificateurs **asynchrones** (peut être utilisé comme un service Angular mais un vrai service est préférable)      
* Modules = state dans le state     

### Utilisation de vuex avec typescript

L'utilisation de vuex avec typescript est un peu complexe et nécessite un peu de configuration :

Créer un fichier *vuex.d.ts* dans le répertoire **src**

````typescript
// vuex.d.ts
import { Store } from '@/vuex';
import { State } from './store'

declare module '@vue/runtime-core' {
  // declare your own store states
  interface State {
    count: number
  }

  // provide typings for `this.$store`
  interface ComponentCustomProperties {
    $store: Store<State>
  }
}

declare module "vuex" {
  export function useStore(key?: string): Store<State>;
}
````

*src/store/index.ts*

````typescript
import { createStore } from "vuex";

export type State = { counter: number };
const state: State = { counter: 0 };

export const store = createStore({
  state: {
    user: null
  },
  getters: {
    getUser(state) {
      return state.user
    }
  },
  mutations: {
    setUser(state, payload) {
      state.user = payload.user || null
    }
  },
  actions: {
  },
  modules: {
  }
})

````

### Utilisation depuis un composant

*App.vue*

````html

<script>
import { ref, onMounted } from 'vue';
import { useStore } from 'vuex';

export default {
  components: {
    Navigation
  },
  setup() {
    const store = useStore();
    
    onMounted(() => {
      console.log('Afficher le state : ', store.state);
      console.log('Appeler le getter : ', store.getters.getUser);
	  
	  // Appeler la mutation 'setUser'
	  store.commit('setUser', session);
      console.log(store.getters.getUser);
    })
  }
}
</script>
````

[Back to top](#ressources-vue3)     
