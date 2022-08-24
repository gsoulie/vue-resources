[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Routing
* [Passage de paramètres en mode props](#passage-de-paramètres-en-mode-props)     
* [Lister les params d'une route](#lister-les-params-d-'-une-route)      
* [Routing dans la vue](#routing-dans-la-vue)    
* [Modifier le titre des pages](#modifier-le-titre-des-pages)    
* [Route guard](#route-guard)      
	
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
[Back to top](#routing)     
	
## Passage de paramètres en mode props

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
[Back to top](#routing)     

## Lister les params d'une route

````typescript
import router from '@/router';

setup() {
	const params = router.currentRoute.value.params
	console.log('params', params);
}
````
[Back to top](#routing)      

## Routing dans la vue

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
[Back to top](#routing)      

## Modifier le titre des pages

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
[Back to top](#routing)      

## Route guard

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

[Back to top](#routing)     
