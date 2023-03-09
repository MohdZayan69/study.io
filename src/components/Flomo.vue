<script setup lang="ts">
import { ref } from "vue";
enum CommandText {
  start = "Start",
  pause = "Pause",
  stop = "Stop",
  resume = "Resume"
}
let commandText = ref(CommandText.start);
let isTimerActive = ref<boolean>(true);
let focusMinutes = ref<number>(0);
let focusSeconds = ref<number>(0);
let breakMinutes = ref<number>(0);
let breakSeconds = ref<number>(0)
let focusTimer: any;
let breakTimer:any;
const handleCommand = () => {
    if(commandText.value === CommandText.start) startFocusTimer()
    else if(commandText.value === CommandText.pause) pauseBreakTimer()
    else if (commandText.value === CommandText.stop) stopFocusTimer()
    else if(commandText.value === CommandText.resume) resumeBreakTimer()
    if(commandText.value == CommandText.start) isTimerActive.value = true
    else if(commandText.value == CommandText.pause || commandText.value == CommandText.stop) isTimerActive.value = false
};
function startFocusTimer(){
    commandText.value = CommandText.stop
    focusTimer = setInterval(()=>{
        if(focusSeconds.value !== 1) focusSeconds.value++
        else{
            focusSeconds.value = 0
            focusMinutes.value++
        }
    }, 1000)
}
function pauseBreakTimer(){
    commandText.value = CommandText.resume
    clearInterval(breakTimer)
}
function resumeBreakTimer(){
    commandText.value = CommandText.pause
    breakTimer = setInterval(()=>{
        if(breakSeconds.value !== 0) breakSeconds.value--
        else if (breakSeconds.value == 0 && breakMinutes.value !== 0) {
            breakMinutes.value--
            breakSeconds.value = 59
        }
        else if(breakMinutes.value ==0 && breakSeconds.value == 0) startFocusTimer()
    }, 1000)
}
function startBreakTimer(){
    breakMinutes.value = Math.floor(focusMinutes.value/5)
    breakSeconds.value = Math.floor(focusSeconds.value/5)
    focusMinutes.value = 0
    focusSeconds.value = 0
    breakTimer = setInterval(()=>{
        if(breakSeconds.value !== 0) breakSeconds.value--
        else if (breakSeconds.value == 0 && breakMinutes.value !== 0) {
            breakMinutes.value--
            breakSeconds.value = 59
        }
        else if(breakMinutes.value ==0 && breakSeconds.value == 0) startFocusTimer()
    }, 1000)
}
function stopFocusTimer(){
    commandText.value = CommandText.pause
    console.log(focusMinutes.value, focusSeconds.value)
    startBreakTimer()
}
</script>
<template>
  <div class="transparent flowmo">
    <h2>Title</h2>
    <h1>{{ commandText === CommandText.stop? `${focusMinutes}:${focusSeconds}`:commandText === CommandText.pause || commandText == CommandText.resume? `${breakMinutes}:${breakSeconds}`: "00:00" }}</h1>
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
