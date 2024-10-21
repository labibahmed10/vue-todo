<script setup lang="ts">
import { ref, onMounted } from "vue";
import Navbar from "./components/Navbar.vue";

interface Todo {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
}

const todoName = ref<string>("");
const todoList = ref<Todo[]>([]);

const onSubmit = () => {
  console.log(todoList.value);
};

onMounted(async () => {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/todos?_limit=5"
  );
  const data = await response.json();
  todoList.value = data;
});
</script>

<template>
  <Navbar />

  <!-- Todo Add form here -->
  <div class="flex items-center justify-center my-10">
    <form class="flex gap-4" @submit.prevent="onSubmit">
      <input
        type="text"
        v-model="todoName"
        placeholder="Add a new todo"
        class="focus:border-0 focus:outline-none focus:ring-2 focus:ring-green-600 border border-solid border-gray-400 p-2 rounded-md"
      />

      <button
        class="bg-indigo-600 rounded-md px-3 text-slate-100 hover:bg-indigo-700 transition-all duration-200"
      >
        Add Todo
      </button>
    </form>
  </div>

  <!-- Todo Lists here -->

  <div
    v-for="todo in todoList"
    :key="todo.id"
    class="flex flex-col items-center justify-center w-1/2 mx-auto"
  >
    <div class="border border-solid border-gray-400 p-2 rounded-md w-full">
      {{ todo.title }}
    </div>
  </div>
</template>
