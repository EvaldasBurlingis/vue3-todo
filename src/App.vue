<template>
<div class="max-w-7xl mx-auto min-h-screen px-4 pb-8 relative">
  <Navbar v-bind="{ toggleModal, isModalOpen }"/>

  <div class="flex flex-col md:flex-row">

    <div class="flex mb-8 md:flex-col md:justify-start md:w-1/4">
      <div class="flex justify-between w-full md:flex-col mb-8">
        <button class="category-btn" @click="filterData('all')">
          <span 
            class="category-btn--circle"
            v-bind:class="[ activeFilter === 'all' ? 'bg-gray-400' : 'bg-gray-200' ]"
            ></span>
          <span>show all</span>
        </button>
        <button class="category-btn" @click="filterData('work')">
          <span 
            class="category-btn--circle"
            v-bind:class="activeFilter === 'work' ? 'bg-indigo-400' : 'bg-indigo-200'"></span>
          <span>work</span>
        </button>
        <button class="category-btn" @click="filterData('study')">
          <span 
            class="category-btn--circle"
            v-bind:class="activeFilter === 'study' ? 'bg-blue-400' : 'bg-blue-200'"></span>
          <span>study</span>
        </button>
        <button class="category-btn" @click="filterData('entertainment')">
          <span 
            class="category-btn--circle"
            v-bind:class="activeFilter === 'entertainment' ? 'bg-red-400' : 'bg-red-200'"></span>
          <span>entertainment</span>
        </button>
        <button class="category-btn" @click="filterData('family')">
          <span 
            class="category-btn--circle"
            v-bind:class="activeFilter === 'family' ? 'bg-green-400' : 'bg-green-200'"></span>
          <span>family</span>
        </button>
      </div>
      <button class="text-gray-500 text-sm text-left" @click="hideDoneTodos = !hideDoneTodos"> 
        <span v-if="!hideDoneTodos">
          Hide done
        </span>
        <span v-else>
          Show all
        </span>
      </button>
    </div>

    <div class="flex-1 grid grid-cols-1 gap-4 md:grid-cols-2 h-full">
      <TodoItem v-for="(todo, todoIndex) in todos" :key="todo.id" v-bind="{todo, todoIndex, deleteTodo, toggleDone, toggleModal, hideDoneTodos }" />
    </div>

    <div class="fixed top-0 left-0 w-full h-full z-10 flex items-center justify-center p-4" v-if="isModalOpen" style="background-color: rgba(255,255,255,0.6)">
      <TodoForm v-bind="{ toggleModal, addNewTodo, selectedTodo, editTodo }"/>
    </div>
  </div>
</div>
</template>

<script>
import { ref } from 'vue';
import Navbar from './components/Navbar';
import TodoItem from './components/TodoItem';
import TodoForm from './components/TodoForm';


export default {
  components: {
    Navbar,
    TodoItem,
    TodoForm
  },
  beforeMount() {
        document.title = "Simple To-do App"
  },
  setup() {
    const isModalOpen = ref(false);
    const selectedTodo = ref(false);
    const hideDoneTodos = ref(false);
    const activeFilter = ref('all');
    const todos = ref([]);

    function addNewTodo(newTodo) {
      todos.value.push({
        id: Date.now(),
        done: false,
        title: newTodo.title,
        content: newTodo.content,
        tags: newTodo.tags,
        hide: false
      })

      selectedTodo.value = false;
    }

    function editTodo(todo) {
      const index = todos.value.indexOf(selectedTodo.value);
      todos.value.splice(index, 1, todo);
    }

    function deleteTodo(index) {
      todos.value.splice(index, 1);
    }

    function filterData(filter) {
      activeFilter.value = filter;

      if (filter === 'all') todos.value.map(t => t.hide = false);

      if (filter !== 'all') {
        todos.value.map(t => {
          if(!t.tags.includes(filter)) {
            t.hide = true; 
          } else {
            t.hide = false;
          }
        })
      }
    }

    function toggleDone(todo) {
       todo.done = !todo.done;
    }

    function toggleModal(todo) {
      if(todo !== false) {
        selectedTodo.value = todo;
      } else {
        selectedTodo.value = false;
      }

      isModalOpen.value = !isModalOpen.value;
    }

    return {
      todos,
      addNewTodo,
      deleteTodo,
      isModalOpen,
      toggleModal,
      selectedTodo,
      editTodo,
      toggleDone, 
      hideDoneTodos,
      activeFilter,
      filterData
    }
  }
}
</script>

<style scoped>
  .category-btn {
    @apply flex items-center text-gray-600 text-sm md:mb-6;
  }

  .category-btn--circle {
    @apply w-6 h-6 block rounded-full mr-1;
  }
</style>
