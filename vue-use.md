[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Vueuse

**VueUse** est une collection de fonctions utilitaires : https://vueuse.org/      

````npm i @vueuse/core````

Exemple : changer le titre de la page dans le browser

````typescript
import { useTitle } from '@vueuse/core';

export default {
    
    setup(props) {
        const title = useTitle();
        title.value = 'MY COOL TITLE';
    }
}
````
