[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Styles

## Ajouter une classe / id css dynamique

````html
<script>
import { ref } from 'vue'

setup() {
	const titleClass = ref('title')
	const myDivId = ref('myDiv')
	return { titleClass, myDivId }
}
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

## Modifier dynamiquement une propriété css

````html
<div class="circle" :style="{color: item.color}"></div>
````
