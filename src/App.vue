<template>
<div class="container mx-auto min-h-screen pt-32">
  <form class="w-full flex justify-center mb-16" @submit.prevent="addNewTodo">
    <input type="text" placeholder="new todo" class="border border-gray-800 py-2 pl-4 focus:outline-none" name="newTodo" v-model="newTodo">
    <button class="bg-gray-800 border border-gray-800 px-4 py-2 text-gray-100 focus:outline-none hover:bg-gray-700">Add</button>
  </form>

  <div>
    <button @click="markAllTodosDone" :class="[todos.length > 0 ? 'inline-block' : 'hidden' ]" class="px-4 py-2 bg-gray-800 text-gray-100 mb-4">Mark all done</button>
    <button @click="markAllTodosUndone" :class="[todos.length > 0 ? 'inline-block' : 'hidden' ]" class="ml-4 px-4 py-2 bg-gray-800 text-gray-100 mb-4">Mark all done</button>
  </div>
  <ul class="list-inside list-disc py-6 px-4 bg-gray-200">
    <li v-for="(todo, index) in todos" :key="todo.id" class="tracking-tight">
      <span class="cursor-pointer" :class="[ todo.done ? 'line-through' : '']" @click="toggleDone(todo)">
        {{ todo.content }}
      </span>
      <button class="text-red-600 ml-8" @click="deleteTodo(index)">X</button>
    </li>
  </ul>

</div>
</template>

<script>
//reactive variable, similar to hooks in react
import { ref } from 'vue';

export default {
  setup() {
    const newTodo = ref('');
    const todos = ref([]);

    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value
      })

      // empty input field
      newTodo.value = '';
    }

    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function deleteTodo(index) {
      // todos.value = todos.value.filter(todoItem => todoItem.id !== id);

      // better vue way
      todos.value.splice(index, 1);
    }

    function markAllTodosDone() {
     todos.value.map(todo => todo.done = true);
    }

    function markAllTodosUndone() {
     todos.value.map(todo => todo.done = false);
    }

    return {
      newTodo,
      todos,
      addNewTodo,
      toggleDone,
      deleteTodo,
      markAllTodosDone,
      markAllTodosUndone
    }
  }
}
</script>

<style>

</style>
