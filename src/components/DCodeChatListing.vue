<script lang="ts" setup>
import type { Chat } from './types';
import { ref, watch } from 'vue';

defineOptions({
  name: "DCodeChatListing"
});

// Define the props for the component
const props = defineProps<{
  chat: Chat;
  selected?: boolean; // Optional prop to indicate if the chat is selected
  ignoreUnread?: boolean; // Optional prop to ignore unread messages
}>();

const localChat = ref<Chat>(props.chat);
const ignoreUnread = ref(props.ignoreUnread === true);

watch(
  () => props.chat,
  (newChat) => {
    localChat.value = newChat;
  }
);


</script>

<template>
    <div class="flex items-center space-x-3 dcode-chat__listing cursor-default" >
        <div class="relative dcode-chat__listing-avatar-container" @click="$emit('selectChat', localChat)">
            <div class="w-10 h-10 rounded-full border dcode-chat__listing-avatar flex items-center justify-center" :class="{ 'border-green-500': selected, 'border-gray-300': !selected }">
                <div v-if="!localChat?.pivot?.chat_avatar" class="w-10 h-10" :class="{ 'fill-green-500': selected, 'fill-gray-300': !selected }">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="currentFill" viewBox="0 0 24 24"><path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/></svg>
                </div>
                <img v-if="localChat?.pivot?.chat_avatar" :src="localChat?.pivot?.chat_avatar + '?selected=' + (selected ? 'selected':'')" :alt="localChat?.pivot?.chat_title" class=" rounded-full object-contain">
            </div>
            <div v-if="localChat?.pivot?.has_new_messages && !ignoreUnread" class="absolute top-0 left-0 w-3 h-3 rounded-full border-2 border-red-500 bg-red-500"></div>
        </div>

        <div class="dcode-chat__listing-text">
            <div class="text-sm font-semibold text-gray-900">
                {{ localChat?.pivot?.chat_title }}
            </div>
            <div class="text-sm text-gray-500">
                {{  localChat?.pivot?.chat_description }}
            </div>
        </div>
    </div>  
</template>

<style scoped>

</style>