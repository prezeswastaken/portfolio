<script setup lang="ts">
import type { Message } from "~/types/types";

definePageMeta({
    middleware: ["admin"],
});

const config = useRuntimeConfig();

const messages = ref<Array<Message> | null>(null);
onMounted(async () => {
    const token = localStorage.getItem("accessToken");
    if (token == null) {
        return navigateTo("/login");
    }
    messages.value = await $fetch<Array<Message>>(
        `${config.public.backendUrl}/message`,
        {
            headers: { Authorization: `Bearer: ${token}` },
        },
    );
});

const messageToDelete = ref<Message | null>(null);
const showDeleteModal = computed(() => messageToDelete == null);
</script>

<template>
    <div>
        <UModal
            v-model:open="showDeleteModal"
            title="Are you sure?"
            description="Lorem ipsum dolor sit amet, consectetur adipiscing elit."
        >
            <template #body>a </template>
        </UModal>
        <div>
            <div>Messages</div>
            <Message v-for="message in messages" :message="message" />
        </div>
    </div>
</template>

<style scoped></style>
