[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Component

## Slot injection

A là manière du *ng-content* d'Angular, il est possible d'utiliser le slot injection avec Vue.

### Utilisation basique

*App.vue*
````html
<template>
  <Child>
      <p>Hello world ! from slot injection</p>
  </Child>
</template>
````

*Child.vue*
````html
<template>
  <slot />
</template>
````

### Portée des évènements du slot

**Attention**, il faut être vigilant sur la portée du scope des éléments d'un slot. Soit le code suivant :

*App.vue*
````html
<template>
  <Child>
    <button @click="presentAlert()">Alert</button>
  </Child>
</template>
````

> Aura pour effet de déclencher la fonction *presentAlert()* du **Parent** ! En effet tout ce qui est à l'intérieur des balises entourant le slot appartient au scope du **parent**

### v-slot
Pour pouvoir utiliser des fonctions du composants enfant depuis le parent il faut utiliser la directive **v-slot**

*App.vue*
````html
<template>
  <Child v-slot="{ data, functionFromChild }">
    <button @click="functionFromChild">press me</button>
    Data from child {{ data.id }} {{ data.user }}
  </Child>
</template>
````

*Child.vue*
````html
<template>
    <div>
        <slot :data="data" :functionFromChild="pressed"/>
    </div>
</template>

<script lang="ts">
export default {
    setup() {
        const data = {
            id: 1,
            user: 'me'
        }
        const pressed = () => { window.alert('child pressed'); }
        return { data, pressed }
    }
}
</script>
````
