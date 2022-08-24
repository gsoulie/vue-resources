[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Framework UI


https://athemes.com/collections/vue-ui-component-libraries/      

**ATTENTION** tous les frameworks ne supportent pas encore Vue3

Ok avec Vue 3 :
* <https://quasar.dev/>     
* https://www.primefaces.org/primevue/setup     
* https://buefy.org/documentation/start     
* https://demos.creative-tim.com/vue-material-kit/documentation/#getting-started      
* https://element-plus.org/en-US/guide/installation.html  

Non encore compatible vue 3 :
* https://vuetifyjs.com/en/getting-started/installation/     

### PrimeVue
````
npm install primevue@^3.12.6 --save
npm install primeicons --save
````

**Importer un thème**

Prime propose une multitude de thème (accessibles depuis le bouton engrenage du side menu https://www.primefaces.org/primevue/setup)

*Exemple import thème par défaut* (imports à ajouter dans le *main.ts*)

````
import 'primevue/resources/themes/saga-blue/theme.css'       //theme
import 'primevue/resources/primevue.min.css'                 //core css
import 'primeicons/primeicons.css'                           //icons
````

**Configuration main.ts**

*main.ts*
````typescript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

import PrimeVue from 'primevue/config'		// importer la config de base PrimeView
import Button from 'primevue/button'		// importer les composants boutons
import InputText from 'primevue/inputtext'	// importer les composants inputs

// importer les thèmes
import 'primevue/resources/themes/saga-blue/theme.css'       //theme
import 'primevue/resources/primevue.min.css'                 //core css
import 'primeicons/primeicons.css'                           //icons


//createApp(App).use(store).use(router).mount('#app')	// à remplacer par :

const app = createApp(App);
app.use(PrimeVue);

// déclarer les composants utilisés
app.component('InputText', InputText);	
app.component('Button', Button);

app.mount('#app');

````
