[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Vuex

## Vue state avec vuex

**vuex** permet de gérer le state de l'application et de répercuter ses modifications automatiquement.

* State = les variables     
* Getter = accesseurs des variables du state     
* Mutations = modificateurs **synchrones** <img src="https://img.shields.io/badge/Important-DD0031.svg?logo=LOGO"> des variables du state     
* Actions = modificateurs **asynchrones** <img src="https://img.shields.io/badge/Important-DD0031.svg?logo=LOGO"> (peut être utilisé comme un service Angular mais un vrai service est préférable)      
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

*main.ts*

````typescript
import { store } from './store'

createApp(App).use(store).use(router).mount('#app')
````

*src/store/index.ts*

````typescript
import { Player } from './../shared/models/player.model';
import { createStore } from 'vuex'

export type State = {
  players: Player[]
}
const state: State = {
  players: []
}

export const store = createStore({
  state,
  getters: {
    getPlayers(state) { return state.players.slice(); }
  },
  mutations: {
    ADD_PLAYER(state, payload: Player) { state.players.push(payload) },
    SET_PLAYERS(state, payload: Player[]) { state.players = payload; },
    REMOVE_PLAYER(state, payload: Player) {
      const playerIndex = state.players.findIndex(p => p.name === payload.name);
      if (playerIndex >= 0) {
        state.players.splice(playerIndex, 1);
      }
    },
    INCREMENT(state, payload: Player) {
      state.players.map(p => {
        if (p.name === payload.name) { p.score = payload.score }
      })
    },
    RESET_SCORE(state) {
      state.players = state.players.map((p: Player) => ({ name: p.name, color: p.color, score: 0 }));
    }
  },
  actions: { },
  modules: { }
})
````

### Utilisation depuis un composant

*App.vue*

````html

<script>
import { useStore } from 'vuex';
import { Player } from '@/shared/models/player.model';

export default {
    name: 'ScoreSheet',
    components: {
        Header,
    },
    setup() {
        const store = useStore();
        const users = computed(() => store.state.players);
        const dummy: Player[] = [
            {
                name: 'Tom',
                score: 301
            },
            {
                name: 'John',
                score: 150
            }
        ];

        onMounted(() => { store.commit('SET_PLAYERS', dummy);})

        function increment(player: Player) {
            player.score += score.value;
            store.commit('INCREMENT', player);
        }
        function newSheet() {
            if (confirm('Êtes-vous certain de vouloir réinitialiser les scores ?')) {
                store.commit('RESET_SCORE');
                score.value = 1;
            }
        }
        function removePlayer(player: Player) {
            if (confirm(`Êtes-vous certain de vouloir supprimer ${ player.name } ?`)) {
                store.commit('REMOVE_PLAYER', player);
            }
        }
        return {...}
    }
}
</script>
````

## Conventions et bonnes pratiques

Les noms des mutations sont en majuscule ````SET_USERS(state, payload)````

Pour accéder au store depuis le composant, il est préconisé d'utiliser une fonction ````computed(()=>{})```` plutot que d'accéder directement au *store.state.value*

````typescript
import { useStore } from 'vuex';
setup() {
        	const store = useStore();
        	const users = computed(() => store.state.players);
	}
````

