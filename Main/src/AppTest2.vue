<script setup>
import { ref, onMounted } from 'vue';

const name = ref('Sebastian');
const status = ref('active');
const tasks = ref(['First task', 'Second task', 'Third task']);
const newTask = ref("");

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending';
  } else if (status.value === 'pending') {
    status.value = 'inactive';
  } else {
    status.value = 'active';
  }
};

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);

  } catch (error) {
    console.log('Error fetching task: $error', {$error: error});
  };
})
</script>

<template>
  <h1 class="text-2xl">Hello {{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

  <h2>Tasks:</h2>
  <ol>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
        <button @click="deleteTask(index)">X</button>
      </span>
    </li>
  </ol>

  <button @click="toggleStatus">Change status</button>
</template>
