<template>
    <article v-if="!todo.hide" class="w-full p-4 rounded-md flex flex-col" :class="[ 
        todo.done ? 'bg-yellow-50' : 'bg-yellow-100',
        hideDoneTodos && todo.done ? 'hidden' : '']">
        <header class="flex justify-between text-gray-700 mb-4">
            <h2 
                class="text-lg font-medium tracking-tight"
                v-bind:class="todo.done && 'line-through text-gray-400'"
                >{{ todo.title }}</h2>
            <div class="relative">
                <IconDotsHorizontal color="text-gray-500 hover:text-gray-700" @click="isMenuOpen = !isMenuOpen"/>    
                <div class="absolute bg-white w-32 rounded-sm shadow-sm flex flex-col text-sm" style="left: -6.5rem" v-if="isMenuOpen">
                    <button class="py-2 border-b border-gray-200 text-gray-400 hover:text-gray-600" @click="openEditModal(todo)">edit ...</button>
                    <button class="py-2 text-red-400 hover:text-red-600" @click="deleteTodo(todoIndex)">delete</button>
                </div>
            </div>
        </header>
        <main class="flex-1 flex flex-col justify-between">
            <p class="text-sm mb-4 leading-6" v-bind:class="todo.done && 'line-through text-gray-400'">{{ todo.content }}</p>
            <div class="flex justify-between">
                <div class="flex">
                    <span 
                        v-for="(tag, index) in todo.tags" 
                        :key="index" 
                        class="w-6 h-6 block rounded-full mr-2"
                        :class="[
                            tag === 'work' && 'bg-indigo-200',
                            tag === 'study' && 'bg-blue-200',
                            tag === 'entertainment' && 'bg-red-200',
                            tag === 'family' && 'bg-green-200'
                        ]"></span>
                </div>
                <IconCheck color="text-gray-500" @click="toggleDone(todo)" v-if="!todo.done"/>
                <IconCheckSolid color="text-gray-500" @click="toggleDone(todo)" v-if="todo.done"/>
            </div>
        </main>
    </article>
</template>

<script>
import { ref } from 'vue';
import IconDotsHorizontal from "./Icons/IconDotsHorizontal";
import IconCheck from "./Icons/IconCheck";
import IconCheckSolid from "./Icons/IconCheckSolid";

export default {
    name: "TodoItem",
    components: {
        IconDotsHorizontal,
        IconCheck,
        IconCheckSolid
    },
    props: ['todo', 'todoIndex', 'deleteTodo', 'toggleDone', 'toggleModal', 'hideDoneTodos'],
    setup(props) {
        const isMenuOpen = ref(false);

        function openEditModal(todo) {
            props.toggleModal(todo);
            isMenuOpen.value = !isMenuOpen.value;
        }

        return {
            isMenuOpen,
            openEditModal
        }
    }

}
</script>