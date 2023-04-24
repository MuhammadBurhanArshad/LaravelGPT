<script setup>
import { computed } from 'vue';
import { Link } from '@inertiajs/vue3';

const props = defineProps(['href', 'active']);

const classes = computed(() =>
    props.active
        ? 'inline-flex items-center px-1 pt-1 border-b-2 border-indigo-400 dark:border-indigo-600 text-sm font-medium leading-5 text-teal-900 dark:text-teal-100 focus:outline-none focus:border-indigo-700 transition duration-150 ease-in-out'
        : 'inline-flex items-center px-1 pt-1 border-b-2 border-transparent text-sm font-medium leading-5 text-teal-500 dark:text-teal-400 hover:text-teal-700 dark:hover:text-teal-300 hover:border-teal-300 dark:hover:border-teal-700 focus:outline-none focus:text-teal-700 dark:focus:text-teal-300 focus:border-teal-300 dark:focus:border-teal-700 transition duration-150 ease-in-out'
);
</script>

<template>
    <Link :href="href" :class="classes">
        <slot />
    </Link>
</template>
