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
  gap: 24px;
  filter: drop-shadow(0px 8px 20px rgba(0, 0, 0, 0.25));
  .timer-content__item {
    flex: 1;
    .timer-content__value {
      font-family: 'Poppins';
      font-weight: 700;
      font-size: 128px;
      line-height: 154px;
      text-align: center;
      color: #F5F5F7;
    }
    .timer-content__description {
      font-family: 'Poppins';
      font-weight: 700;
      font-size: 24px;
      line-height: 29px;
      text-align: center;
      color: #F5F5F7;
    }
  }
  @media (max-width: 1150px) {
    gap: 12.5px;
    .timer-content__item {
      .timer-content__value {
        font-size: 54px;
      }
    }
  }
  @media (max-width: 768px) {
    .timer-content__item {
      .timer-content__value {
        font-size: 24px;
        line-height: 34px;
      }
      .timer-content__description {
        font-size: 10px;
      }
    }
  }
}
</style>
