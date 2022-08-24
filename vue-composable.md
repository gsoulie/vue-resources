[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Composable

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
