<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head } from "@inertiajs/vue3";
</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <!-- <template #header>
            <h2
                class="font-semibold text-xl text-teal-800 dark:text-teal-200 leading-tight"
            >
                Dashboard
            </h2>
        </template> -->

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div
                    class="bg-white dark:bg-teal-800 overflow-hidden shadow-sm sm:rounded-lg text-center"
                >
                    <div class="p-6 text-teal-900 dark:text-teal-100">
                        <h1 class="text-xl font-bold">
                            Welcome {{ $page.props.auth.user.name }}!
                        </h1>
                        <h4>You are logged In</h4>
                            <a class="underline" :href="route('chat.show')">Try Chat</a>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
