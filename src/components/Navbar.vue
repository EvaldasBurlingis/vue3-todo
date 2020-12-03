<template>
    <nav class="flex w-full justify-between items-center py-4 md:mb-8 relative">
        <span class="text-2xl font-semibold text-gray-500">to-do</span>
         <IconPlus size="8" color="text-gray-500 cursor-pointer hover:text-gray-700" @click="toggleModal(false)" v-if="!isModalOpen"/>
    </nav>
</template>

<script>

import IconPlus from "./Icons/IconPlus";

export default {
    name: 'Navbar',
    props: ['toggleModal', 'isModalOpen'],
    components: {
        IconPlus
    }
}
</script>