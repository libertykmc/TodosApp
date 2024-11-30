<script setup lang="ts">
import {ref, computed} from 'vue'

let id = 0;

const newTodo = ref('');
const todos = ref([
  {id: id++, text:'Test Todo', done: false},
])
const addTodo = () => {
  todos.value.push({id: id++, text: newTodo.value, done: false});
  newTodo = '';
}
const removeTodo = (todo) => {
todos.value.filter((t)=>t!==todo)
}
</script>

<template>
  <div class="container">
    
      <input type="text" placeholder="Add Todo" v-model="newTodo">
      <button @click="addTodo">Add</button>
  
    <ul v-for="todo in todos" :key="todo.id">{{ todo.text }}
      <button @click="removeTodo(todo)">delete</button>
    </ul>
  </div>
</template>

<style scoped>
.container {
  min-height: 100dvh;
}
</style>