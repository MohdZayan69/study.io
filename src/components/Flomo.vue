<script setup lang="ts">
import { ref } from "vue";
enum CommandText {
  start = "Start",
  pause = "Pause",
}
let commandText = ref(CommandText.start);
let isTimerActive = ref<boolean>(true);
let focusMinutes = ref<number>(0);
const handleCommand = () => {
    if(commandText.value !== CommandText.start) commandText.value = CommandText.start
    else if(commandText.value !== CommandText.pause) commandText.value = CommandText.pause
    if(commandText.value == CommandText.start) isTimerActive.value = true
    else if(commandText.value == CommandText.pause) isTimerActive.value = false
};
</script>
<template>
  <div class="transparent flowmo">
    <h2>Title</h2>
    <h1>00:00</h1>
    <div class="btn-con">
      <button @click="handleCommand">{{ commandText }}</button>
      <button :disabled="isTimerActive">Reset</button>
    </div>
  </div>
</template>
<style scoped>
.flowmo {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: 16px;
}
.btn-con {
  margin-top: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
}
</style>
