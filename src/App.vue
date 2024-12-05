<template>
  <div class="app">
    <h1>Clicker App</h1>
    <p>Tap Count: {{ counter }}</p>
    <button @click="incrementCounter">Tap Me!</button>
  </div>
</template>

<script lang="ts">
import { ref, onMounted } from "vue";

export default {
  name: "App",
  setup() {
    // Локальное состояние счетчика
    const counter = ref<number>(0);

    // Получение текущего значения счетчика с сервера
    const fetchCounter = async () => {
      try {
        const response = await fetch("/get_counter");
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const data = await response.json();
        counter.value = data.taps;
      } catch (error) {
        console.error("Failed to fetch counter:", error);
      }
    };

    // Увеличение значения счетчика
    const incrementCounter = async () => {
      counter.value += 1;
      try {
        const response = await fetch("/update_counter", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({ taps: counter.value }),
        });
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
      } catch (error) {
        console.error("Failed to update counter:", error);
      }
    };

    // Загружаем начальное значение счетчика при монтировании
    onMounted(() => {
      fetchCounter();
    });

    return {
      counter,
      incrementCounter,
    };
  },
};
</script>

<style>
.app {
  text-align: center;
  margin-top: 50px;
}

button {
  padding: 10px 20px;
  font-size: 18px;
  cursor: pointer;
}

button:hover {
  background-color: #4caf50;
  color: white;
}
</style>
