<script setup>
import { ref, onMounted } from "vue";

const tasks = ref([]);
const newTask = ref("");
const loading = ref(true);

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    await new Promise((resolve) => setTimeout(resolve, 2000));
    const res = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await res.json();
    data.forEach((item) => {
      tasks.value.push(item.title);
    });
  } catch (error) {
    console.log("error fetching tasks", error);
  } finally {
    loading.value = false;
  }
});
</script>

<template>
  <div class="px-4 pt-10 max-w-lg mx-auto">
    <div v-if="loading" class="flex justify-center items-center space-x-2">
      <span>Loading tasks...</span>
    </div>
    <div v-else>
      <h3 class="text-2xl font-semibold text-gray-900 mb-6">Tasks:</h3>
      <!-- Display tasks -->
      <ul class="space-y-3">
        <li
          v-for="(task, index) in tasks"
          :key="task"
          class="flex justify-between items-center bg-gray-800 text-white px-4 py-3 rounded-lg shadow-md transition-all duration-200"
        >
          <span>{{ task }}</span>
          <button
            @click="removeTask(index)"
            class="text-sm cursor-pointer text-rose-500 w-6 h-6 bg-rose-500/10"
          >
            X
          </button>
        </li>
        <li v-if="tasks.length === 0" class="text-gray-500 text-sm">
          No tasks yet. Add a new one!
        </li>
      </ul>

      <!-- Task Input Form -->
      <form @submit.prevent="addTask" class="space-y-4 mt-6">
        <div class="flex flex-col gap-2">
          <label for="newTask" class="text-sm text-gray-300">Task Name:</label>
          <input
            type="text"
            id="newTask"
            placeholder="Enter a new task"
            v-model="newTask"
            class="px-4 py-2 bg-gray-800 text-gray-200 border border-transparent rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-50 w-full"
          />
        </div>
        <button
          type="submit"
          :disabled="newTask.trim() === ''"
          class="w-full px-4 py-2 bg-indigo-600 text-sm text-white rounded-lg shadow-md hover:bg-indigo-700 transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-50 disabled:opacity-50"
        >
          Add Task
        </button>
      </form>
    </div>
  </div>
</template>
