<template>
  <div class="container">
    <div class="todo-app">

      <h1>Kegiatan-Ku! <br> Hal yang harus dilakukan</h1>
      <div class="row">
        <input type="text" v-model="newTask" placeholder="Ketik kegiatan">
        <button @click="addTask">ADD</button>
      </div>

      <div class="filters">
        <button @click="hideCompleted = !hideCompleted" class="button-74">

          {{ hideCompleted ? 'tampil' : 'sembunyi' }}
        </button>
      </div>




      <ul>
        <li v-for="(task, index) in filteredTodos" :key="task.id" :class="{ checked: task.checked }" @click="toggleTask(task)">
          {{ task.text }}
          <span @click.stop="removeTask(task)">&#xd7;</span>
        </li>
      </ul>
    </div>


  </div>

</template>

<script setup>


import { ref, computed } from 'vue';

const newTask = ref('');

const tasks = ref([]);
const hideCompleted = ref(false);
const muted = ref(false);

const volume = ref(1);
const filteredTodos = computed(() => {
  return hideCompleted.value
    ? tasks.value.filter((task) => !task.checked)
    : tasks.value;
});
function addTask() {
  if (newTask.value.trim() === '') {
    alert("Isi terlebih dahulu!");
  } else {
    tasks.value.unshift({ id: Date.now(), text: newTask.value, checked: false });
    newTask.value = '';
    saveData();
  }
}
function toggleTask(task) {
  task.checked = !task.checked; 
  saveData();
}
function removeTask(task) {
  const index = tasks.value.findIndex((t) => t.id === task.id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
    saveData();
  }
}
function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}
function loadData() {
  const savedTasks = localStorage.getItem("tasks");
  tasks.value = savedTasks ? JSON.parse(savedTasks) : [];
}
loadData();
</script>
<style scoped>
.container {
  background: rgb(165, 35, 212);
  width: 80%;
  min-height: 50vh;
  padding: 20px;
  margin-top: 20px;
  margin-left: -10px;
  margin-bottom: 20px;
}

.todo-app {
  width: 100%;
  max-width: 600px;
  background: plum;
  margin: 0 auto 40px;
  padding: 10px 40px 70px;
}
.todo-app h1{
  text-align: right;
  color:black;
  margin-bottom: 30px;
}

.row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgb(162, 93, 172);
  padding-left: 20px;
  border-radius: 20px;
}

input {
  flex: 2;
  border: 1px;
  outline: 1px;
  background: #424242;
  padding: 8px;
  font-size: 16px;
  font-weight: 200px;
  color: #000000;
}

.row button {
  outline: none;
  padding: 15px 15px;
  background: azure;
  color: #fff;
  font-size: 15px;
  cursor: pointer;
}
.row button:hover{
  background:bottom;
}

ul li {
  list-style: none;
  font-size: 45px;
  padding: 10px 10px 4px 45px;
  user-select: none;
  cursor: pointer;
  position: relative;
  color:black;
}

ul li::before {
  content: '';
  position: absolute;
  height: 35px;
  width: 35px;
  border-radius: 20%;
  background-image: url(./assets/unchecked.png);
  background-size: cover;
  background-position: center;
  top: 30px;
  left: 5px;
}

ul li.checked {
  color:rgb(51, 44, 94);
  text-decoration: line-through;
}

ul li.checked::before {
  background-image: url(assets/checked.png);
}

ul li span {
  border-radius: 30px;
  position: absolute;
  right: 1;
  top: 30px;
  width: 35px;
  height: 35px;
  font-size: 35px;
  color: #9b9b9b;
  line-height: 30px;
  text-align: center;
}

ul li span:hover {
  background: rgb(90, 90, 167);
}

.filters{
display: flex;
justify-content: center;
padding-top: 5vh;
  
}

.filters button{
  width: 20px;
}


.button-74 {
  background-color: #9b9b9b;
  color: rgb(65, 26, 65);
  cursor: pointer;
  display: inline-block;
  font-weight: 300;
  font-size: 15px;
  padding: 0 45px;
  line-height: 35px;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-74:hover {
  background-color: #424242;
}

@media (min-width: 800px) {
  .button-74 {
    min-width: 120px;
    padding: 0 35px;
  }
}
</style>