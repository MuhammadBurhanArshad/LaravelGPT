<script setup>
defineProps(['value']);
</script>

<template>
    <label class="block font-medium text-sm text-teal-700 dark:text-teal-300">
        <span v-if="value">{{ value }}</span>
        <span v-else><slot /></span>
    </label>
</template>
