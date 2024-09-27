<template>
  <div class="chat-container">
    <ChatWindow :messages="messages" :user="1" @sendMessage="sendMessage" />
    <ChatWindow :messages="messages" :user="2" @sendMessage="sendMessage" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import ChatWindow from './components/ChatWindow.vue';

const messages = ref([]);

const sendMessage = (message, user) => {
  messages.value.push({ text: message, user });
};
</script>
