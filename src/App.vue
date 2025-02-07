<script setup>
import { ref, computed } from 'vue';

const tasks = ref([
  { id: 1, name: 'Task 1', completed: true },
  { id: 2, name: 'Task 2', completed: false },
  { id: 3, name: 'Task 3', completed: false },
]);
const newTask = ref(''); // To bind to the input field

const addTask = () => {
  if (newTask.value.trim() !== '') {
    const newId = tasks.value.length + 1;
    tasks.value.push({ id: newId, name: newTask.value, completed: false });
    newTask.value = ''; // Clear input field after adding the task
  }
};

const deleteTask = (taskId) => {
  tasks.value = tasks.value.filter(task => task.id !== taskId);
};

const pendingTasks = computed(() => tasks.value.filter(task => task.completed == false));
const completedTasks = computed(() => tasks.value.filter(task => task.completed == true));
</script>

<template>
  <header>
    <h1>Just Do It.</h1>
    <div>
      <input type="text" placeholder="Add a new task" v-model="newTask"/>
      <button @click="addTask">Add</button>
    </div>
  </header>

  <main>
    <!-- Show Pending Tasks (not completed tasks) -->
    <ul v-if="pendingTasks.length > 0">
      <li v-for="task in pendingTasks" :key="task.id">
        <span>
          <input type="checkbox" v-model="task.completed"/>
          <label>{{ task.name }}</label>
        </span>
        <div>
          <button>Edit</button>
          <button @click="deleteTask(task.id)"> <i class="fas fa-trash"></i></button>
        </div>
      </li>
    </ul>

    <!-- Show message when all tasks are completed -->
    <div v-if="tasks.every(task => task.completed)">
      All Tasks have been completed!
    </div>

    <!-- Show Completed Tasks -->
    <ul v-if="completedTasks.length > 0">
      <li v-for="task in completedTasks" :key="task.id">
        <span>
          <input type="checkbox" v-model="task.completed"/>
          <label>{{ task.name }}</label>
        </span>
        <div>
          <button>Edit</button>
          <button @click="deleteTask(task.id)"> <i class="fas fa-trash"></i></button>
        </div>
      </li>
    </ul>
  </main>
</template>
