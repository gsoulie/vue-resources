# Ressources Vue3

* [Bases](https://github.com/gsoulie/vue-resources/blob/main/vue-basics.md)      
* [Presentation](#presentation)      
* [Events](#emit-events)     
* [Watcher](#watcher)     
* [Routing](#routing)     


## Présentation

https://www.jesuisundev.com/comprendre-vuejs-en-5-minutes/      

VueJS est un framework Javascript frontend pour créer des interfaces utilisateurs. Tu vas me dire « encore un ? » et la réponse est oui. Sauf qu’il est un peu différent.

Déjà, c’est intéressant de comprendre que VueJS a été conçu pour être intégré de façon incrémentale. Ça veut dire que **si tu as une application frontend déjà existante, t’as pas à tout refaire. Tu peux faire une nouvelle partie en VueJS et l’intégrer rapidement au reste**.

VueJS prend aussi les bonnes idées de ses concurrents. Il permet le data binding. Les données et le DOM sont couplés et réactifs aux changements. On retrouve également le concept de virtual dom avec VueJS. Le DOM n’est pas directement changé, ça passe par le virtual DOM.

On retrouve aussi l’organisation par composant. Cette fonctionnalité permet de découper ton application en plusieurs sous-composants qui gèrent chacun leur vie et qui sont réutilisables. Imaginons tu veux faire une liste d’images : tu peux faire un composant qui gère l’image et un composant qui gère une liste de composant image.

VueJS se concentre sur la partie vue de ton application. Pour ce faire, le framework s’inspire en partie du patron d’architecture MVVM. VueJS va lier ton DOM, la partie vue, avec ton instance de vue, la partie Vue-Modèle. Ces deux parties sont liées par le système de data-binding

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


### Solution 2 - composition API : émettre depuis le script

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
    emits: ["customChange"],
    setup(props, { emit }) {
        
		const sendChange = (event) => {
			emit("customChange", event.target.value)
		}
		
        return {
            sendChange
        }
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

### Lister les params d'une route

````typescript
import router from '@/router';

setup() {
	const params = router.currentRoute.value.params
	console.log('params', params);
}
````

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
