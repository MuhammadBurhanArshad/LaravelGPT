<script setup>
import { Link } from '@inertiajs/vue3';
</script>

<template>
    <Link
        class="block w-full px-4 py-2 text-left text-sm leading-5 text-teal-700 dark:text-teal-300 hover:bg-teal-100 dark:hover:bg-teal-800 focus:outline-none focus:bg-teal-100 dark:focus:bg-teal-800 transition duration-150 ease-in-out"
    >
        <slot />
    </Link>
</template>
