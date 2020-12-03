<template>
    <form @submit.prevent class="bg-white shadow-2xl w-full p-8 max-w-2xl relative">
        <div class="flex justify-end">
            <IconPlus size="8" color="text-gray-500 rotate-45 transform hover:text-gray-700 cursor-pointer" @click="toggleModal(false)"/>
        </div>
        
        <div class="mb-4">
            <label for="title" class="label">Title</label>
            <input type="text" name="title" id="title" class="input" placeholder="Enter Title" v-model="form.title">
        </div>

        <div class="mb-4">
            <label for="title" class="label">Content</label>
            <input type="text" name="title" id="title" class="input" placeholder="Enter Title" v-model="form.content">
        </div>

        <div class="mb-4">
            <label for="title" class="label mb-4 block">Tags</label>
            <div class="grid grid-cols-2 gap-2 md:grid-cols-4">
                <button class="category-btn" @click="toggleTag('work')">
                    <span 
                        class="category-btn--circle"
                        v-bind:class="form.tags.includes('work') ? 'bg-indigo-400' : 'bg-indigo-200'"
                        ></span>
                    <span>work</span>
                </button>
                <button class="category-btn" @click="toggleTag('study')">
                    <span 
                    class="category-btn--circle"
                    v-bind:class="form.tags.includes('study') ? 'bg-blue-400' : 'bg-blue-200'"></span>
                    <span>study</span>
                </button>
                <button class="category-btn" @click="toggleTag('family')">
                    <span class="category-btn--circle"
                    v-bind:class="form.tags.includes('family') ? 'bg-green-400' : 'bg-green-200'"></span>
                    <span>family</span>
                </button>
                <button class="category-btn" @click="toggleTag('entertainment')">
                    <span 
                        class="category-btn--circle"
                        v-bind:class="form.tags.includes('entertainment') ? 'bg-red-400' : 'bg-red-200'"></span>
                    <span>entertainment</span>
                </button>
            </div>
        </div>

        <button class="text-white bg-green-500 px-4 py-1 text-sm rounded-sm" @click="saveTodo">
            <span v-if="!selectedTodo">
                Create
            </span>
            <span v-else>
                Edit
            </span>
        </button>


    </form >
</template>

<script>

import { ref } from "vue";
import IconPlus from "./Icons/IconPlus"

export default {
    name: 'TodoForm',
    components: {
        IconPlus
    },
    props: ['toggleModal', 'addNewTodo', 'editTodo', 'selectedTodo'],
    setup(props) {
        const form = ref({
            id: Date.now(),
            title: props.selectedTodo ? props.selectedTodo.title : '',
            content: props.selectedTodo ? props.selectedTodo.content : '',
            tags: props.selectedTodo ? props.selectedTodo.tags : [],
            done: props.selectedTodo ? props.selectedTodo.done : false
        })

        function toggleTag(tag) {
            if(form.value.tags.includes(tag)) {
                form.value.tags.splice(form.value.tags.indexOf(tag), 1)
            } else {
                form.value.tags.push(tag);
            }
        }

        function saveTodo() {
            if (props.selectedTodo) {
                props.editTodo(form.value);
            } else {
                props.addNewTodo(form.value)
            }
            props.toggleModal(false);
        }

        return {
            form,
            saveTodo,
            toggleTag
        }
    }
}
</script>

<style scoped>
    .label {
        @apply text-xs text-gray-900;
    }

    .input {
        @apply border border-gray-400 w-full rounded-sm py-2 px-4 text-sm;
    }

    .category-btn {
        @apply flex items-center text-gray-400 text-sm md:mb-6;
    }

  .category-btn--circle {
    @apply w-6 h-6 block rounded-full mr-1;
  }
</style>