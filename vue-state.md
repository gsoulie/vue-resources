[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Vuex

## Vue state avec vuex

**vuex** permet de gérer le state de l'application et de répercuter ses modifications automatiquement.

* State = les variables     
* Getter = accesseurs des variables du state     
* Mutations = modificateurs **synchrones*** [<img src="https://img.shields.io/badge/Important-DD0031.svg?logo=LOGO">] des variables du state     
* Actions = modificateurs **asynchrones (IMPORTAN)** (peut être utilisé comme un service Angular mais un vrai service est préférable)      
* Modules = state dans le state     

### Utilisation de vuex avec typescript

L'utilisation de vuex avec typescript est un peu complexe et nécessite un peu de configuration :

Créer un fichier *vuex.d.ts* dans le répertoire **src**

````typescript
// vuex.d.ts
import { Store } from '@/vuex';
import { State } from './store'

declare module '@vue/runtime-core' {
  // declare your own store states
  interface State {
    count: number
  }

  // provide typings for `this.$store`
  interface ComponentCustomProperties {
    $store: Store<State>
  }
}

declare module "vuex" {
  export function useStore(key?: string): Store<State>;
}
````

*src/store/index.ts*

````typescript
import { createStore } from "vuex";

export type State = { counter: number };
const state: State = { counter: 0 };

export const store = createStore({
  state: {
    user: null
  },
  getters: {
    getUser(state) {
      return state.user
    }
  },
  mutations: {
    setUser(state, payload) {
      state.user = payload.user || null
    }
  },
  actions: {
  },
  modules: {
  }
})

````

### Utilisation depuis un composant

*App.vue*

````html

<script>
import { ref, onMounted } from 'vue';
import { useStore } from 'vuex';

export default {
  components: {
    Navigation
  },
  setup() {
    const store = useStore();
    
    onMounted(() => {
      console.log('Afficher le state : ', store.state);
      console.log('Appeler le getter : ', store.getters.getUser);
	  
	  // Appeler la mutation 'setUser'
	  store.commit('setUser', session);
      console.log(store.getters.getUser);
    })
  }
}
</script>
````
