[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Props et Events

* [Props](#props)     
* [Events](#events)     

## Props

Les **props** sont comparables aux ````@Input()```` d'Angular. Elles permettent de partager des variables avec d'autres composants

Il est officiellement recommandé de déclarer les props de la manière suivante :

````typescript
// Simply
props: {
  status: String
}

// Even better!
props: {
  status: {
    type: String,
    required: true,

    validator: value => {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].includes(value)
    }
  }
}
````

De même qu'il est **recommandé** d'utiliser la syntaxe *camelCase* dans le code de déclaration et la syntaxe *kebab-case* dans le template

````html
<script>
props: {
  greetingText: String
}
</script>

<template>
	<WelcomeMessage greeting-text="hi"/>
</template>
````

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

        onMounted(() => { initializeDatabase(); });
        
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
[Back to top](#props-et-events)      

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
[Back to top](#props-et-events)      

## Events
  
> équivalent @Output() Angular

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
    placeholder="Que cherchez vous ?"/>
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
	
[Back to top](#props-et-events)      

