<template>
    <div class="transparent flowmo">
      <h2>{{ commandText === CommandText.focus ? `Focus Session ${sessionCount}`: commandText === CommandText.break ? "Break" : "Long Break"}}</h2>
      <h1>
        {{ commandText === CommandText.focus ? `${focusMinutes
            .toString()
            .padStart(2, '0')}:${focusSeconds.toString().padStart(2, '0')}` :
          commandText === CommandText.break ? `${breakMinutes
            .toString()
            .padStart(2, '0')}:${breakSeconds.toString().padStart(2, '0')}` :
          `${longBreakMinutes.toString().padStart(2, '0')}:${longBreakSeconds
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
    longbreak = 'Long Break'
  }
  
  let commandText = ref(CommandText.focus);
  let isTimerActive = ref<boolean>(false);
  let focusMinutes = ref<number>(0);
  let focusSeconds = ref<number>(0);
  let breakMinutes = ref<number>(0);
  let breakSeconds = ref<number>(0);
  let focusTimer: any;
  let breakTimer: any;
  let longBreakTimer: any;
  let totalFocusSession = ref<number>(0);
  let sessionCount = ref<number>(1);
  let longBreakMinutes = ref<number>(0);
  let longBreakSeconds = ref<number>(0);
  
  const startFocusSession = () => {
    commandText.value = CommandText.focus;
    focusTimer = setInterval(() => {
      if (focusSeconds.value !== 59) {
        focusSeconds.value++;
        totalFocusSession.value++;
      } else {
        focusSeconds.value = 0;
        focusMinutes.value++;
      }
    }, 1000);
    isTimerActive.value = true;
  };
  
  const stopSession = () => {
    clearInterval(focusTimer);
    clearInterval(breakTimer);
    console.log(totalFocusSession.value)
    isTimerActive.value = false;
    sessionCount.value++;
    if (commandText.value === CommandText.focus) {
      commandText.value = CommandText.break;
      const previousFocusSessionLength = focusMinutes.value * 60 + focusSeconds.value;
      breakMinutes.value = Math.floor(previousFocusSessionLength / 5 / 60);
      breakSeconds.value = Math.floor((previousFocusSessionLength / 5) % 60);
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
    if (sessionCount.value === 4) startLongBreakSession();
    else {
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
    }
  };
  
  const startLongBreakSession = () => {
    commandText.value = CommandText.longbreak
    sessionCount.value = 1;
    longBreakMinutes.value = Math.floor(totalFocusSession.value/300)
    longBreakSeconds.value = Math.floor((totalFocusSession.value/5)%5)
    longBreakTimer = setInterval(()=>{
        if(longBreakSeconds.value > 0 ){
            longBreakSeconds.value--
        }
        else if(longBreakMinutes.value > 0){
            longBreakMinutes.value--
            longBreakSeconds.value = 59
        }
        else{
            clearInterval(longBreakTimer)
            commandText.value = CommandText.focus
            totalFocusSession.value = 0
            alert("Done")
        }
    }, 1000)
}

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
  