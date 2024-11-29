<script setup>

import { onMounted, ref } from 'vue';

const name = ref("John Doe");
const status = ref("yoo");
const tasks = ref(['task 1', 'task 2', 'task 3']);
const link = ref("https://google.com");
const newTask = ref("");
const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};

const handleSubmit = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map(task => task.title);
  } catch (error) {
    console.error("Error fetching tasks:", error);
  }
});

</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'"> User is active</p>
  <p v-else-if="status === 'pending'"> User is pending </p>
  <p v-else> User is inactive </p>
  <form @submit.prevent="handleSubmit">
    <input type="text" label='newTask' id='newTask' v-model="newTask" />
    <button type="submit">Submit</button>
  </form>
  <h3>Task:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <a :href="link">click to go away</a>
  <button @click="toggleStatus">click to toggle status</button>
</template>
