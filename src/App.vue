<script setup lang="ts">
import {ref, computed} from 'vue'

let id = 0;

const hideCompleted = ref(false)
const newTodo = ref('');
const todos = ref([
  {id: id++, text:'Test Todo', done: false},
])
const addTodo = () => {
  todos.value.push({id: id++, text: newTodo.value, done: false});
  newTodo.value = '';
}
const removeTodo = (todo) => {
todos.value = todos.value.filter((t)=>t!==todo)
}
const filteredTodos = computed(() => {
  return hideCompleted.value? todos.value.filter((t)=> !t.done) : todos.value
})
</script>

<template>
  <div class="container">
    
      <input type="text" placeholder="Add Todo" v-model="newTodo">
      <button @click="addTodo">Add</button>
  
    <ul v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }} </span>
      <button @click="removeTodo(todo)">delete</button>
    </ul>
    <button @click="hideCompleted = !hideCompleted">{{ hideCompleted? 'Показать все' : 'Скрыть выполненные' }}</button>
  </div>
</template>

<style scoped>
.container {
  min-height: 100dvh;
}
.done {
  text-decoration: line-through;
}
</style>