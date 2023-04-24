<script setup>
import { ref, onMounted, computed } from "vue";
import { Head } from "@inertiajs/vue3";
import ChatLayout from "@/Layouts/ChatLayout.vue";
import { useForm, Link } from "@inertiajs/vue3";
import ChatContent from "@/Components/ChatContent.vue";
import Skeleton from "@/Components/Skeleton.vue";

const promtInput = ref(null);
const chatContainer = ref(null);
const showDeleteButton = ref(false);

const props = defineProps({
    messages: Array,
    chat: null | Object,
});

const form = useForm({
    promt: "",
});

const submit = () => {
    const url = props.chat ? `/chat/${props.chat?.id}` : "/chat";
    form.post(url, {
        onFinish: () => clear(),
    });
};

const scrollToBottom = () => {
    if (props.chat) {
        const el = chatContainer.value;
        el.scrollTop = el.scrollHeight;
    }
};

const clear = () => {
    form.promt = "";
    promtInput.value.focus();
    scrollToBottom();
};
onMounted(() => {
    clear();
});

const title = computed(() => props.chat?.context[0].content ?? "New Chat");
</script>
<template>
    <Head :title="title" />
    <ChatLayout>
        <template #aside>
            <ul class="p-2">
                <li
                    v-if="chat"
                    class="px-4 py-2 my-2 flex justify-between font-semibold text-yellow-400 bg-teal-700 hover:bg-yellow-400 hover:text-teal-700 rounded-lg duration-200"
                >
                    <Link href="/chat" class="w-full text-center"
                        >New Chat</Link
                    >
                </li>
                <template v-for="message in messages" :key="message.id">
                    <li
                        :class="[
                            message.id === chat?.id
                                ? 'bg-teal-700'
                                : 'bg-teal-900',
                            'px-4 py-2 my-2 flex justify-between font-semibold text-yellow-400 hover:bg-yellow-400 hover:text-teal-700 rounded-lg duration-200',
                        ]"
                    >
                        <Link :href="`/chat/${message.id}`">{{
                            message.context[0].content
                        }}</Link>
                        <div v-if="message.id === chat?.id">
                            <button
                                v-if="!showDeleteButton"
                                @click="showDeleteButton = !showDeleteButton"
                            >
                                <svg
                                    xmlns="http://www.w3.org/2000/svg"
                                    version="1.1"
                                    xmlns:xlink="http://www.w3.org/1999/xlink"
                                    xmlns:svgjs="http://svgjs.com/svgjs"
                                    x="0"
                                    y="0"
                                    viewBox="0 0 512 512"
                                    style="enable-background: new 0 0 512 512"
                                    xml:space="preserve"
                                    class="h-4 w-4"
                                >
                                    <g>
                                        <path
                                            d="m62.205 150 26.569 320.735C90.678 493.865 110.38 512 133.598 512h244.805c23.218 0 42.92-18.135 44.824-41.265L449.795 150H62.205zm118.781 302c-7.852 0-14.458-6.108-14.956-14.063l-15-242c-.513-8.276 5.771-15.395 14.033-15.908 8.569-.601 15.381 5.757 15.908 14.033l15 242c.531 8.57-6.25 15.938-14.985 15.938zM271 437c0 8.291-6.709 15-15 15s-15-6.709-15-15V195c0-8.291 6.709-15 15-15s15 6.709 15 15v242zm89.97-241.062-15 242c-.493 7.874-7.056 14.436-15.908 14.033-8.262-.513-14.546-7.632-14.033-15.908l15-242c.513-8.276 7.764-14.297 15.908-14.033 8.262.513 14.546 7.632 14.033 15.908zM451 60h-90V45c0-24.814-20.186-45-45-45H196c-24.814 0-45 20.186-45 45v15H61c-16.569 0-30 13.431-30 30 0 16.567 13.431 30 30 30h390c16.569 0 30-13.433 30-30 0-16.569-13.431-30-30-30zm-120 0H181V45c0-8.276 6.724-15 15-15h120c8.276 0 15 6.724 15 15v15z"
                                            fill="#ff0000"
                                            data-original="#000000"
                                            class=""
                                        ></path>
                                    </g>
                                </svg>
                            </button>
                            <span
                                v-if="showDeleteButton"
                                class="flex justify-between"
                            >
                                <Link
                                    :href="route('chat.destroy', chat?.id)"
                                    method="DELETE"
                                    as="button"
                                    class="text-red-300 hover:text-red-500"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        fill="none"
                                        viewBox="0 0 24 24"
                                        stroke-width="1.5"
                                        stroke="currentColor"
                                        class="w-6 h-6"
                                    >
                                        <path
                                            stroke-linecap="round"
                                            stroke-linejoin="round"
                                            d="M4.5 12.75l6 6 9-13.5"
                                        />
                                    </svg>
                                </Link>

                                <button
                                    @click="showDeleteButton = false"
                                    class="text-teal-300 hover:text-teal-500"
                                >
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        fill="none"
                                        viewBox="0 0 24 24"
                                        stroke-width="1.5"
                                        stroke="currentColor"
                                        class="w-6 h-6"
                                    >
                                        <path
                                            stroke-linecap="round"
                                            stroke-linejoin="round"
                                            d="M6 18L18 6M6 6l12 12"
                                        />
                                    </svg>
                                </button>
                            </span>
                        </div>
                    </li>
                </template>
            </ul>
        </template>
        <div class="w-full flex text-white">
            <template v-if="chat">
                <div class="w-full flex h-screen bg-teal-900">
                    <div class="w-full overflow-auto pb-36" ref="chatContainer">
                        <template
                            v-for="(content, index) in chat?.context"
                            :key="index"
                        >
                            <ChatContent :content="content" />
                        </template>
                        <Skeleton v-show="form.processing" />
                    </div>
                </div>
            </template>
        </div>
        <template #form>
            <section class="px-6 top-0">
                <div class="w-full">
                    <div class="relative flex-1 flex items-center">
                        <input
                            type="text"
                            class="w-full bg-teal-700 text-white rounded-lg"
                            placeholder="Assalamulikum Ask me anything!"
                            v-model="form.promt"
                            @keyup.enter="submit"
                            :disabled="form.processing"
                            ref="promtInput"
                        />
                        <div
                            class="absolute inset-y-0 right-0 flex items-center pl-3"
                        >
                            <svg
                                v-if="!form.processing"
                                xmlns="http://www.w3.org/2000/svg"
                                fill="none"
                                viewBox="0 0 24 24"
                                stroke-width="1.5"
                                stroke="currentColor"
                                class="w-6 h-6 -ml-8 text-teal-200"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    d="M6 12L3.269 3.126A59.768 59.768 0 0121.485 12 59.77 59.77 0 013.27 20.876L5.999 12zm0 0h7.5"
                                />
                            </svg>
                            <div
                                class="dot-typing -ml-10"
                                v-if="form.processing"
                            ></div>
                        </div>
                    </div>
                </div>
            </section>
        </template>
    </ChatLayout>
</template>
<style>
.dot-typing {
    position: relative;
    left: -9999px;
    width: 10px;
    height: 10px;
    border-radius: 5px;
    background-color: #7bad77;
    color: #7bad77;
    box-shadow: 9984px 0 0 0 #7bad77, 9999px 0 0 0 #7bad77,
        10014px 0 0 0 #7bad77;
    animation: dot-typing 1.5s infinite linear;
}
@keyframes dot-typing {
    0% {
        box-shadow: 9984px 0 0 0 #7bad77, 9999px 0 0 0 #7bad77,
            10014px 0 0 0 #7bad77;
    }
    16.667% {
        box-shadow: 9984px -10px 0 0 #7bad77, 9999px 0 0 0 #7bad77,
            10014px 0 0 0 #7bad77;
    }
    33.333% {
        box-shadow: 9984px 0 0 0 #7bad77, 9999px 0 0 0 #7bad77,
            10014px 0 0 0 #7bad77;
    }
    50% {
        box-shadow: 9984px 0 0 0 #7bad77, 9999px -10px 0 0 #7bad77,
            10014px 0 0 0 #7bad77;
    }
    66.667% {
        box-shadow: 9984px 0 0 0 #7bad77, 9999px 0 0 0 #7bad77,
            10014px 0 0 0 #7bad77;
    }
    83.333% {
        box-shadow: 9984px 0 0 0 #7bad77, 9999px 0 0 0 #7bad77,
            10014px -10px 0 0 #7bad77;
    }
    100% {
        box-shadow: 9984px 0 0 0 #7bad77, 9999px 0 0 0 #7bad77,
            10014px 0 0 0 #7bad77;
    }
}
</style>
