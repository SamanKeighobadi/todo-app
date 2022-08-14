<template>
  <h1>hellow world</h1>
  <input type="text" placeholder="what is your todo ?" v-model="name" />

  <form @submit.prevent="addTodo">
    <h4>What is on your todo list?</h4>
    <input type="text" placeholder="todo list" v-model="input_content" />

    <input type="submit" value="Add Todo" />
  </form>

  {{ todos }}
</template>

<script setup>
import { ref, onMounted, computed, watch } from "vue";
import { createToast } from 'mosha-vue-toastify';
import 'mosha-vue-toastify/dist/style.css'

const todos = ref([{title:"saman",computed:false,createdAt: new Date().getTime(),}]);
const name = ref("");
const input_content = ref("");


const addTodo = () => {
  if (input_content.value.trim() === "") {
    return;
  }



  todos.value.push({
    title: input_content.value,
    completed: false,
    createdAt: new Date().getTime(),
  });



  watch(
    todos,
    (newTodo) => {
      localStorage.setItem("todos", JSON.stringify(newTodo));
      console.log(newTodo);
    },
    {
      deep: true,
    }
  );
};


watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || " ";
  if(localStorage.getItem('todos')){

    todos.value = JSON.stringify(localStorage.getItem('todos')) || [];
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
