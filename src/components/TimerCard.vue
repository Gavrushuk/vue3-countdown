<script lang="ts" setup>
import { ref, toRefs, watch } from 'vue';
import TimerContent from './TimerContent.vue';
import TimerEnd from "@/assets/timer-end.mp3";

const props = defineProps<{
  title: string,
  date: Date
}>();

const { date } = toRefs(props);

const audio = new Audio(TimerEnd);
audio.volume = 0.5;
const isTimeFinish = ref(false);

const days = ref("00");
const hours = ref("00");
const minutes = ref("00");
const seconds = ref("00");
let timerInterval: number;

function resetTimer() {
  clearInterval(timerInterval);
  days.value = "00";
  hours.value = "00";
  minutes.value = "00";
  seconds.value = "00";
}

function updateTimer() {
  if ((props.date.getTime() - (new Date()).getTime()) <= 0) {
    resetTimer();
  }

  const timerTime = props.date.getTime() - (new Date()).getTime();

  const daysCount = Math.floor(timerTime / (24 * 60 * 60 * 1000));
  const daysms = timerTime % (24 * 60 * 60 * 1000)
  days.value = daysCount < 10 ? '0' + daysCount : String(daysCount);
  
  const hoursCount = Math.floor(daysms / (60 * 60 * 1000));
  const hoursms = timerTime % (60 * 60 * 1000);
  hours.value = hoursCount < 10 ? '0' + hoursCount : String(hoursCount);
  
  const minutesCount = Math.floor(hoursms / (60 * 1000));
  const minutesms = timerTime % (60 * 1000);
  minutes.value = minutesCount < 10 ? '0' + minutesCount : String(minutesCount);
  
  const secondsCount = Math.floor(minutesms / 1000);
  seconds.value = secondsCount < 10 ? '0' + secondsCount : String(secondsCount);
}

function startTimer() {
  if ((props.date.getTime() - (new Date()).getTime()) <= 0) return;

  timerInterval = window.setInterval(() => {
    updateTimer();

    if ((props.date.getTime() - (new Date()).getTime()) <= 0) {
      isTimeFinish.value = true;
      resetTimer();
      audio.play();
      setTimeout(() => {
        audio.pause();
        isTimeFinish.value = false;
      }, 30000);
      return;
    }

    if (isTimeFinish.value) {
      audio.pause();
      isTimeFinish.value = false;
    }
  }, 1000);
}

watch(date, () => {
  resetTimer();
  startTimer();
}, { immediate: true });
</script>

<template>
  <div
    class="timer-card"
    :class="{
      finish: isTimeFinish
    }"
  >
    <div class="timer-title">{{ title }}</div>
    <TimerContent
      :days="days"
      :hours="hours"
      :minutes="minutes"
      :seconds="seconds"
    />
  </div>
</template>

<style lang="scss" scoped>
.timer-card {
  width: 100%;
  max-width: 1000px;
  cursor: pointer;
  &.finish {
    animation: shake 1s;
    animation-iteration-count: infinite;
  }
  .timer-title {
    font-weight: 500;
    font-size: 64px;
    line-height: 77px;
    text-align: center;
    color: #F5F5F7;
    overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
    filter: drop-shadow(0px 7px 20px #343946);
  }
  .timer-content {
    margin-top: 25px;
  }
  @media (max-width: 768px) {
    .timer-title {
      font-size: 40px;
      line-height: 60px;
    }
  }
}

@keyframes shake {
  0% { transform: translate(1px, 1px) rotate(0deg); }
  10% { transform: translate(-1px, -2px) rotate(-1deg); }
  20% { transform: translate(-3px, 0px) rotate(1deg); }
  30% { transform: translate(3px, 2px) rotate(0deg); }
  40% { transform: translate(1px, -1px) rotate(1deg); }
  50% { transform: translate(-1px, 2px) rotate(-1deg); }
  60% { transform: translate(-3px, 1px) rotate(0deg); }
  70% { transform: translate(3px, 1px) rotate(-1deg); }
  80% { transform: translate(-1px, -1px) rotate(1deg); }
  90% { transform: translate(1px, 2px) rotate(0deg); }
  100% { transform: translate(1px, -2px) rotate(-1deg); }
}
</style>
