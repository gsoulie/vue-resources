[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Généralités

* [Présentation](#presentation)    
* [Script setup](#script-setup)     
* [ref() vs reactive()](#ref-vs-reactive)     
* [Commandes](#commandes)      
* [Extensions VSCode](#extensions-vscode)   
* [Initialisation projet](#initialisation-projet)       
* [Raccourcis](#raccourcis)     
* [Lifecycle hook](#lifecycle-hook)      
* [Custom font](#custom-font)     

## Présentation

https://www.jesuisundev.com/comprendre-vuejs-en-5-minutes/      

VueJS est un framework Javascript frontend pour créer des interfaces utilisateurs. Tu vas me dire « encore un ? » et la réponse est oui. Sauf qu’il est un peu différent.

Déjà, c’est intéressant de comprendre que VueJS a été conçu pour être intégré de façon incrémentale. Ça veut dire que **si tu as une application frontend déjà existante, t’as pas à tout refaire. Tu peux faire une nouvelle partie en VueJS et l’intégrer rapidement au reste**.

VueJS prend aussi les bonnes idées de ses concurrents. Il permet le data binding. Les données et le DOM sont couplés et réactifs aux changements. On retrouve également le concept de virtual dom avec VueJS. Le DOM n’est pas directement changé, ça passe par le virtual DOM.

On retrouve aussi l’organisation par composant. Cette fonctionnalité permet de découper ton application en plusieurs sous-composants qui gèrent chacun leur vie et qui sont réutilisables. Imaginons tu veux faire une liste d’images : tu peux faire un composant qui gère l’image et un composant qui gère une liste de composant image.

VueJS se concentre sur la partie vue de ton application. Pour ce faire, le framework s’inspire en partie du patron d’architecture MVVM. VueJS va lier ton DOM, la partie vue, avec ton instance de vue, la partie Vue-Modèle. Ces deux parties sont liées par le système de data-binding

Le code présenté ici utilise l'api *composition* et la syntaxe *SFC (Single File Component)* ainsi que l'utilisation du script ````<script setup>````

La fonctionnalité principale de Vue est le **rendu déclaratif** : en utilisant une syntaxe de modèle qui étend le HTML, nous pouvons décrire à quoi le HTML devrait ressembler en fonction de l'état de JavaScript. Lorsque l'état change, le HTML se met à jour automatiquement

[Back to top](#généralités)     

## Script setup

Le script ````setup```` permet de s'affranchir de la syntaxe return qui permet d'exposer les variables / fonctions à la vue

Deux syntaxes sont alors possibles :

*Avec script setup*

````typescript
<script setup lang="ts">
import DropDown from './DropDown.vue';
import { defineProps } from 'vue';

const props = defineProps({
    showMenu: Boolean
})

function setHello() {...}
</script>
````

*Sans script setup*

````typescript
<script lang="ts">
import DropDown from './DropDown.vue';

export default {
    name: 'Header',
    components: {
        DropDown
    },
    props: {
        showMenu: Boolean
    },
    setup() {
		function setHello() {...}
		
		return { setHello }
	}
}
</script>
````

[Back to top](#généralités)     

## ref() vs reactive()

L'état qui peut déclencher des mises à jour lorsqu'il est modifié est considéré comme **reactive**. Nous pouvons déclarer l'état réactif en utilisant l'API ````reactive()```` de Vue. Les objets créés à partir de ````reactive()```` sont des proxies JavaScript qui fonctionnent comme des objets normaux

````reactive()```` ne fonctionne que sur les objets (y compris les tableaux et les types intégrés comme Map et Set). ````ref()````, d'autre part, peut prendre n'importe quel type de valeur et créer un objet qui expose la valeur interne sous une propriété ````.value````

Les variables qui ne sont pas utilisées dans la vue n'ont pas besoin d'être déclarée comme ````reactive()```` ou ````ref()````.

Il est **conseillé** de préférer l'utilisation de ````ref()```` qui présente plus d'avantage que *reactive()*

En résumé :

**ref**
* Fonctionne sur les types primitifs et objets
* peut être réaffecté

**reactive**
* Ne nécessite pas l'utilisation du .value 
* Ne fonctionne **pas** sur les types primitifs (number, string, boolean) ex : ````const value = reactive(3); // lève une erreur console````
* Ne peut pas être réaffecté

````typescript
let cube = reactive({
        length: 10,
        height: 20,
        width: 30
});

const update = () => {
    cube = {
        length: 9,
        height: 99,
        width: 999
    };
	// NE FONCTIONNE PAS
}
````

**ref, isRef, unref, reactive, toRef, toRefs, customRef, ...**
https://www.youtube.com/watch?v=sAj6tdVS2cA&ab_channel=ProgramWithErik

[Back to top](#généralités)     

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
````typescript
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build --mode production"	// <-- rajouter le mode de build. commande dans le terminal : $vue build
  },
````

*Obtenir un projet complètement configuré avec routing etc...*

````
npm init vue@latest
cd <project_directory>
npm i
npm  run dev
````

[Back to top](#généralités)     

## Extensions VSCode

* vetur (snippet)    
* vue 3 snippets highlight formatters     

[Back to top](#généralités)     

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
[Back to top](#généralités)     

## Raccourcis

| Directive        | Raccourcis           |
| ------------- |:-------------:|
|````v-bind:<value>````|````:<value>````|
|````v-bind:class````|````:class````|
|````v-bind:id````|````:id````|
|````v-on:click````|````@click````|
|````v-on:<event_source>````|````@<event_source>````|
|````v-on:input````|````@input````|

[Back to top](#généralités)     

## Lifecycle hook

https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram      

* onMounted = ngOnInit()      
* onUpdated      
* onUnmounted = ngOnDestroy()     

Jusqu'à présent, Vue gérait pour nous toutes les mises à jour du DOM, grâce à la réactivité et au rendu déclaratif. Cependant, il y aura inévitablement des cas où nous devrons travailler manuellement avec le DOM.

Il est possible de d'obtenir une référence à un élément du template grâce à l'attribut **ref()**

> Notez que la référence est initialisée avec une valeur nulle. C'est parce que l'élément n'existe pas encore lorsque <script setup> est exécuté. La référence du modèle n'est accessible qu'après le montage du composant.

````html
<script>
import { ref, onMounted } from 'vue'

setup() {
	const p = ref(null)
	onMounted(() => { p.value.textContent = 'mounted!' })
}
</script>

<template>
  <p ref="p">hello</p>
</template>
````
  
[Back to top](#généralités)     

## Custom font

1 - uploader les fonts dans le répertoire **/public/fonts**
	
2 - Configurer le fichier **/public/index.html** pour pré-charger les fonts
	
*index.html*
````html
<head>
	<link rel="preload" as="font" href="./fonts/TitilliumWeb-Regular.ttf" type="font/ttf" crossorigin="anonymous">
</head>
````

3 - Fichier **styles.scss**

Déclarer ensuite les fonts dans le fichier de style global
	
````css
	@font-face {
    font-family: "TitilliumWeb-Regular";
    src: local("TitilliumWeb-Regular"), url("/public/fonts/TitilliumWeb-Regular.ttf") format("truetype");
    font-weight: 200;
    font-style: normal;
    font-display: swap;
}

body {
    margin: 0;
    font-family: 'TitilliumWeb-Regular', Helvetica, Arial, sans-serif;
    background-color: #f0f0f0;
}  
button {
	font-family: 'TitilliumWeb-Regular', Helvetica, Arial, sans-serif;	// IMPORTANT sinon non pris en compte sur les boutons
}
````
[Back to top](#généralités)     
