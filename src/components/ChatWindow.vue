<template>
  <div :class="['chat-window']">
    {{'User ' + user}}
    <div ref="messagesContainer" class="messages">
      <div
          v-for="(msg, index) in messages"
          :key="index"
          :class="{
          'message-container-sent': msg.user === user,
          'message-container-received': msg.user !== user,
          'message-container': true,
          'no-margin-top': index > 0 && msg.user === messages[index - 1].user
        }"
      >
        <div v-if="shouldShowSender(index)" class="message-sender">
          {{ msg.user === user ? 'User ' + msg.user : 'User ' + msg.user }}
        </div>
        <div class="message">
          {{ msg.text }}
        </div>
      </div>
    </div>
    <div class="input-container">
      <input
          v-model="newMessage"
          placeholder="Поле для ввода сообщений"
          @keydown.enter="handleSend"
      />
      <button @click="handleSend" class="send-button">Отправить</button>
    </div>
  </div>
</template>

<script setup>
import { ref, watch,nextTick  } from 'vue';

const props = defineProps({
  messages: Array,
  user: Number,
});

const newMessage = ref('');
const emit = defineEmits(['sendMessage']);
const messagesContainer = ref(null);

const handleSend = () => {
  if (newMessage.value.trim()) {
    emit('sendMessage', newMessage.value, props.user);
    newMessage.value = '';
    nextTick(() => {
      scrollToBottom();
    });
  }
};

const shouldShowSender = (index) => {
  if (index === 0) {
    return true;
  }
  return props.messages[index].user !== props.messages[index - 1].user;
};

const scrollToBottom = () => {
  if (messagesContainer.value) {
    messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight;
  }
};

watch(
    () => props.messages,
    () => {
      scrollToBottom();
    }
);
</script>
