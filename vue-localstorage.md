[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Local Storage

L'utilisation du local storage avec Vue est plutôt simple, il suffit d'utiliser ````localStorage.setItem('<nom_item>', value)```` et ````localStorage.getItem('<nom_item>')````

Voici un exemple de todo list basique : 

````typescript
<script lang="ts">

import { ref, onMounted, computed, watch } from 'vue';

export interface ITodo {
  content: string,
  category: string,
  done: boolean,
  createdAt: number
}

export default {
  setup() {
    const todos = ref<ITodo[]>([]);
    const name = ref('');
    const input_content = ref('');
    const input_category = ref(null);

    onMounted(() => {
      // Charger les données depuis le local storage
      name.value = localStorage.getItem('name') || '';
      const res = localStorage.getItem('todos') || '';
      todos.value = JSON.parse(res) || [];
    })
    
    const addTodos = () => {
      if (input_content.value.trim() === '' || input_category.value === null) {
        alert('Please enter a value and select a category')
        return;
      }
      todos.value.push({
        content: input_content.value,
        category: input_category.value,
        createdAt: Date.now(),
        done: false
      });
      input_content.value = '';
      input_category.value = null;
    }

    const removeTodo = (todo: ITodo) => {
      if (!confirm(`Are you sure to delete ${todo.content} ?`)) { return; }
      
      todos.value = todos.value.filter((t: ITodo) => t !== todo);
    }

    const todos_asc = computed(() => todos.value.sort((a: ITodo, b: ITodo) => b.createdAt - a.createdAt));

    // Observer les changements de la variable "name" et mettre à jour le local storage
    watch(name, (newVal: string) => {
      localStorage.setItem('name', newVal);
    });
    
    // Observer les changements de la liste todos et mettre à jour le local storage
    watch(todos, (newVal: ITodo[]) => {
      localStorage.setItem('todos', JSON.stringify(newVal));
    }, { deep: true });
    
    return {
      name,
      todos,
      todos_asc,
      input_content,
      input_category,
      addTodos,
      removeTodo
    }
  }
}
</script>
<template>
  <div>
    <main class="container">
      <section class="greeting">
        <h2 class="title">
          Hello <input type="text" placeholder="username" v-model="name" />
        </h2>
      </section>

      <section class="create-todos">
        <h3>Create todo</h3>

        <form @submit.prevent="addTodos">
          <h4>What's your todo ?</h4>
          <input type="text" placeholder="e.g. make a video" v-model="input_content">
          <h4>Pick a category</h4>
          <div class="row">
            <div class="options">
              <label>
                <input 
                type="radio" 
                name="category"
                value="business" 
                v-model="input_category">
                <span class="bubble business"></span>
                <div>Business</div>
              </label>
            </div>
            <div class="flex"></div>
            <div class="options">
              <label>
                <input 
                type="radio" 
                name="category"
                value="personal" 
                v-model="input_category">
                <span class="bubble business"></span>
                <div>Personal</div>
              </label>
            </div>
          </div>
          <br>
          <button class="btn" type="submit">Add todo</button>
        </form>
      </section>
      <section class="todo-list">
        <h3>TODO LIST</h3>
        <div class="list">
          <div v-for="todo in todos" 
          class="item"
          :key="todo">
            <label class="space-between">
              <div>
                <input type="checkbox" v-model="todo.done"/>
                <span class="bubble" :class="todo.done ? 'done-item' : 'undone-item'">
                  {{ todo.content }}
                </span>
              </div>
              <button class="btn-delete" @click="removeTodo(todo)">Delete</button>
            </label>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>
````
