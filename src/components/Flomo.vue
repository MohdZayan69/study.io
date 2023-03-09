<template>
    <div class="transparent flowmo">
      <h2>Title</h2>
      <h1>
        {{ commandText === CommandText.focus ? `${focusMinutes
        .toString()
        .padStart(2, '0')}:${focusSeconds.toString().padStart(2, '0')}` :
        `${breakMinutes.toString().padStart(2, '0')}:${breakSeconds
        .toString()
        .padStart(2, '0')}` }}
      </h1>
      <div class="btn-con">
        <button @click="startFocusSession" :disabled="isTimerActive">
          Start
        </button>
        <button @click="stopSession" :disabled="!isTimerActive">
          Stop
        </button>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  
  enum CommandText {
    focus = 'Focus',
    break = 'Break',
  }
  
  let commandText = ref(CommandText.focus);
  let isTimerActive = ref<boolean>(false);
  let focusMinutes = ref<number>(0);
  let focusSeconds = ref<number>(0);
  let breakMinutes = ref<number>(0);
  let breakSeconds = ref<number>(0);
  let focusTimer: any;
  let breakTimer: any;
  
  const startFocusSession = () => {
    commandText.value = CommandText.focus;
    focusTimer = setInterval(() => {
      if (focusSeconds.value !== 59) focusSeconds.value++;
      else {
        focusSeconds.value = 0;
        focusMinutes.value++;
      }
    }, 1000);
    isTimerActive.value = true;
  };
  
  const stopSession = () => {
    clearInterval(focusTimer);
    clearInterval(breakTimer);
    isTimerActive.value = false;
    if (commandText.value === CommandText.focus) {
      commandText.value = CommandText.break;
      breakMinutes.value = Math.floor(focusMinutes.value / 5);
      breakSeconds.value = Math.floor(focusSeconds.value / 5);
      focusMinutes.value = 0;
      focusSeconds.value = 0;
      startBreakSession();
    } else {
      commandText.value = CommandText.focus;
      focusMinutes.value = 0;
      focusSeconds.value = 0;
    }
  };
  
  const startBreakSession = () => {
    breakTimer = setInterval(() => {
      if (breakSeconds.value > 0) {
        breakSeconds.value--;
      } else if (breakMinutes.value > 0) {
        breakMinutes.value--;
        breakSeconds.value = 59;
      } else {
        clearInterval(breakTimer);
        startFocusSession();
      }
    }, 1000);
  };
  </script>
  
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
  