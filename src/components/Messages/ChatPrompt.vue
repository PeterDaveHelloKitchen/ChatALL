<template>
  <v-card
    ref="root"
    class="message prompt"
    :class="isThread ? 'thread-prompt' : ''"
  >
    <pre>{{ message ? message.content : "" }}</pre>
    <v-btn flat size="x-small" icon @click="copyToClipboard" class="copy-button">
      <v-icon>mdi-content-copy</v-icon>
    </v-btn>
  </v-card>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";

const root = ref();
const props = defineProps({
  message: {
    type: Object,
    required: true,
  },
  columns: {
    type: Number,
    required: true,
  },
  isThread: {
    type: Boolean,
    default: false,
  },
});

watch(
  () => props.columns,
  () => {
    root.value.$el.style.setProperty("--columns", props.columns);
  },
);

onMounted(() => {
  root.value.$el.style.setProperty("--columns", props.columns);
});

function copyToClipboard() {
  navigator.clipboard.writeText(props.message.content);
}
</script>

<style scoped>
.message {
  border-radius: 8px;
  padding: 16px;
  word-wrap: break-word;
  text-align: left;
  position: relative;
}

.prompt {
  background-color: rgb(var(--v-theme-prompt));
  width: fit-content;
  grid-column: 1 / span var(--columns);
}

.prompt pre {
  white-space: pre-wrap;
  font-family: inherit;
}

.thread-prompt {
  width: 100%;
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.copy-button {
  position: absolute;
  top: 0;
  right: -40px;
}
</style>
