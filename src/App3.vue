<script setup>
import { onMounted, ref } from 'vue';

  const name = ref('John Doe');
  const status = ref('active');
  const tasks = ref(['Tasks 1', 'Tasks 2','Tasks 3']);
  const newTask = ref('');

  const toggleStatus = () => {
    if(status.value === 'active'){
      status.value = 'pending';
    }else if(status.value === 'pending'){
      status.value = 'inactive';
    }else{
      status.value = 'active';
    }
  }

  const addTask = () => {
    if (newTask.value.trim() !== ''){
      tasks.value.push(newTask.value);
      newTask.value = '';
    }
  }

  const deleteTasks = (index) => {
    tasks.value.splice(index,1);
  }

  onMounted(async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((task) => task.title);
    }catch (error){
      console.log("Error Fetching Tasks");
    }
  });

</script>

<template>
  <h1>
    {{ name }}
  </h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User in Inactive</p>

   <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask"/>
    <button tyoe="submit">Submit</button>
   </form>
  <h3>Tasks:</h3>
  <ul>
    <li 
      v-for="(task, index) in tasks"
      :key="tasks">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTasks(index)">x</button>
    </li>
  </ul>
  <br/>
  <button @click="toggleStatus">
    Change Status
  </button>
</template>

<style scoped>
  /* h1 {
    color:red;
  } */
</style>
