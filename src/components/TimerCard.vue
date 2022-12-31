<script lang="ts" setup>
import { onMounted, ref, toRefs, watch } from 'vue';
import TimerContent from './TimerContent.vue';
import HappyNewYear from "@/assets/happy-new-year.mp3";

const props = defineProps<{
  title: string,
  date: Date
}>();

const { date } = toRefs(props);

const audio = new Audio(HappyNewYear);
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
  timerInterval = window.setInterval(() => {
    updateTimer();
    if ((props.date.getTime() - (new Date()).getTime()) <= 0) {
      isTimeFinish.value = true;
      resetTimer();
      audio.play();
      setTimeout(() => {
        audio.pause();
        isTimeFinish.value = false;
      }, 58000);
    }
  }, 1000);
}

watch(date, () => {
  resetTimer();
  startTimer();
});

onMounted(() => {
  resetTimer();
  startTimer();
});
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
  background: rgba(9, 33, 67, 0.8);
  box-shadow: 0px 0px 0px 0px transparent;
  backdrop-filter: blur(10px);
  border-radius: 36px;
  width: 100%;
  max-width: 1050px;
  padding: 50px 86px;
  &.finish {
    animation: light-show 5s, shake 1s;
    animation-iteration-count: infinite;
  }
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

@keyframes light-show {
  0% { box-shadow: 0px 0px 0px 0px transparent; }
  10% { box-shadow: 0px 0px 500px 100px #0ca90c; }
  20% { box-shadow: 0px 0px 0px 0px transparent; }
  30% { box-shadow: 0px 0px 500px 100px #ce0d0d; }
  40% { box-shadow: 0px 0px 0px 0px transparent; }
  50% { box-shadow: 0px 0px 500px 100px #ffd700; }
  60% { box-shadow: 0px 0px 0px 0px transparent; }
  70% { box-shadow: 0px 0px 500px 100px #3225de; }
  80% { box-shadow: 0px 0px 0px 0px transparent; }
  90% { box-shadow: 0px 0px 500px 100px #8314b9; }
  100% { box-shadow: 0px 0px 0px 0px transparent; }
}
</style>
