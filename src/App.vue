<script setup lang="ts">
import { ref, computed } from "vue";

const loadTodos = (): { text: string; done: boolean }[] => {
  const storedTodos = localStorage.getItem("todos");
  return storedTodos ? JSON.parse(storedTodos) : [];
};

const hideCompleted = ref(false);
const newTodo = ref("");
const todos = ref<{ text: string; done: boolean }[]>(loadTodos());

const saveTodos = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value.trim(), done: false });
    newTodo.value = "";
    saveTodos();
  }
};

const removeTodo = (todo: { text: string; done: boolean }) => {
  todos.value = todos.value.filter((t) => t !== todo);
  saveTodos();
};

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});
</script>

<template>
  <div class="container">
    <h1>Write your plans here</h1>
    <div class="inputholder">
      <input
        class="input"
        type="text"
        placeholder="Add Todo"
        v-model="newTodo"
      />
      <button @click="addTodo">Add</button>
    </div>
    <ul>
      <li class="list" v-for="todo in filteredTodos" :key="todo.text">
        <input class="input" type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)">Delete</button>
      </li>
    </ul>

    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? "Показать все" : "Скрыть выполненные" }}
    </button>
  </div>
</template>

<style scoped>
.container {
  min-height: 100dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

h1 {
  color: #2e69f7;
  margin-bottom: 20px;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.inputholder {
  display: flex;
  gap: 8px;
}

.input {
  padding: 5px;
  border-radius: 20px;
  color: #2e69f7;
  text-align: center;
}

.list {
  display: flex;
  gap: 8px;
  align-items: center;
}

button {
  background: linear-gradient(135deg, #6a11cb, #2575fc);
  border: none;
  color: white;
  padding: 12px 24px;
  font-size: 16px;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
}

button:hover {
  background: linear-gradient(135deg, #2575fc, #6a11cb);
  transform: translateY(-2px);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15), 0 2px 4px rgba(0, 0, 0, 0.12);
}

button:active {
  transform: translateY(1px);
  box-shadow: 0 3px 5px rgba(0, 0, 0, 0.2), 0 1px 2px rgba(0, 0, 0, 0.15);
}

button:focus {
  outline: none;
  box-shadow: 0 0 0 4px rgba(37, 117, 252, 0.3);
}
</style>
