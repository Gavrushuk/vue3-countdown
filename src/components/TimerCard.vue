<script lang="ts" setup>
import { ref, toRefs, watch } from 'vue';
import TimerContent from './TimerContent.vue';

const props = defineProps<{
  title: string,
  date: Date
}>();

const { date } = toRefs(props);

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
  timerInterval = window.setInterval(() => {
    updateTimer();
    if ((props.date.getTime() - (new Date()).getTime()) <= 0) {
      resetTimer();
    }
  }, 1000);
}

watch(date, () => {
  resetTimer();
  startTimer();
});
</script>

<template>
  <div class="timer-card">
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
  background: rgba(9, 33, 67, 0.8);
  box-shadow: 24px 26px 4px -11px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  border-radius: 36px;
  width: 100%;
  max-width: 1050px;
  padding: 50px 86px;
  .timer-title {
    font-weight: 200;
    font-size: 90px;
    line-height: 135px;
    text-align: center;
    letter-spacing: 0.125em;
    color: #FFFFFF;
    overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
  }
  .timer-content {
    margin-top: 25px;
  }
  @media (max-width: 1150px) {
    padding: 50px;
    .timer-title {
      font-size: 70px;
      line-height: 90px;
    }
  }
  @media (max-width: 768px) {
    padding: 12.5px;
    .timer-title {
      font-size: 40px;
      line-height: 60px;
    }
  }
}
</style>
