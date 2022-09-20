[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Composable

Un code partagé entre plusieurs composant peut être extrait dans un troisième composant qui sera appelé dans chacun des autres composants ayant besoin de ce même code. Avec Vue, une autre solution existe, les **Composables**. Les Composables permettent de partager du code *controller* à la manière d'un *service* Angular

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

## Autre exemple

````typescript
import { ref, toRaw } from 'vue';

export function useSort(data) {
	const sortedItems = ref([])
	
	const sortByStrings = (criteria) => {
		return data.sort((a, b) => {
			if(a[criteria] < b[criteria]) return -1;
			if(a[criteria] > b[criteria]) return 1;
			return 0;
		});
	}
	
	const sortByNumbers = (criteria) => {
		return data.sort((a, b) => a[criteria] - b[criteria]);
	}
	
	const sortByDates = (criteria) => {
		return data.sort((a, b) => {
			return new Date(a[criteria]) - new Date(b[criteria])
		});
	}
	
	
	function sortItemBy(criteria) {
		const type = typeof toRaw(data)[0][criteria];
		
		if (type === 'string') return sortByStrings(criteria)
		if (type === 'number') return sortByNumbers(criteria)
		if (type === 'object') return sortByDates(criteria)
	}
	
	return { sortedItems, sortItemBy }
}
````
