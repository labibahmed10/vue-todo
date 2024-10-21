<script setup lang="ts">
import { ref, onMounted } from "vue";
import Navbar from "./components/Navbar.vue";
import { EpDelete } from "vue-icons-plus/ep";

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

const checkTodo = (id: number) => {
  todoList.value = todoList.value.map((todo) => {
    if (todo.id === id) {
      return {
        ...todo,
        completed: !todo.completed,
      };
    }
    return todo;
  });
};

onMounted(async () => {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/todos?_limit=3"
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
  {{ console.log(todoList) }}
  <div
    v-for="todo in todoList"
    :key="todo.id"
    class="flex flex-col items-center justify-center w-2/3 mx-auto"
  >
    <div
      class="bg-slate-50 border border-solid border-green-400 p-2 rounded-md w-full my-2 flex justify-between items-center"
    >
      <div class="space-x-2">
        <input
          class="size-4 text-blue-500 focus:ring-blue-500"
          type="checkbox"
          :id="'todo-' + todo.id"
          v-model="todo.completed"
        />
        <label
          :for="'todo-' + todo.id"
          class="text-gray-800"
          :class="{ 'line-through': todo.completed }"
          >{{ todo.title }}</label
        >
      </div>

      <EpDelete class="size-6 text-red-600 cursor-pointer" />
    </div>
  </div>
</template>
