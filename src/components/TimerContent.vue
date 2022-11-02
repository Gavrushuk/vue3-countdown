<script lang="ts" setup>
import { toRefs } from 'vue';

const props = defineProps<{
  days: string;
  hours: string;
  minutes: string;
  seconds: string;
}>();

const { days, hours, minutes, seconds } = toRefs(props);
</script>

<template>
  <div class="timer-content">
    <div class="timer-content__item">
      <div class="timer-content__value">{{ days || 0 }}</div>
      <div class="timer-content__description">DAYS</div>
    </div>
    <div class="timer-content__item">
      <div class="timer-content__value">{{ hours || 0 }}</div>
      <div class="timer-content__description">HOURS</div>
    </div>
    <div class="timer-content__item">
      <div class="timer-content__value">{{ minutes || 0 }}</div>
      <div class="timer-content__description">MINUTES</div>
    </div>
    <div class="timer-content__item">
      <div class="timer-content__value">{{ seconds || 0 }}</div>
      <div class="timer-content__description">SECONDS</div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.timer-content {
  display: flex;
  align-items: center;
  gap: 26px;
  .timer-content__item {
    flex: 1;
    padding: 46px 29px;
    background: #080F1A;
    border-radius: 22px;
    .timer-content__value {
      font-family: 'Poppins';
      font-weight: 900;
      font-size: 64px;
      line-height: 86px;
      text-align: center;
      color: #FFFFFF;
    }
    .timer-content__description {
      font-family: 'Poppins';
      font-weight: 300;
      font-size: 16px;
      line-height: 21px;
      text-align: center;
      color: #A5DEFF;
    }
  }
}
</style>
