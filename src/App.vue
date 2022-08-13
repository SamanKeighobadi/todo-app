<template>
  <h1>hellow world</h1>
  <input type="text" placeholder="what is your todo ?" v-model="name" />

  <form @submit.prevent="addTodo">
    <h4>What is on your todo list?</h4>
    <input type="text" placeholder="todo list" v-model="input_content" />

    <input type="submit" value="Add Todo" />
  </form>

  {{ input_content }}
</template>

<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");

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
    },
    {
      deep: true,
    }
  );
};

const input_content = ref("");
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || " ";
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
