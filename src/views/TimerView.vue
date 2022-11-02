<script setup lang="ts">
import TimerCard from '@/components/TimerCard.vue';
</script>

<template>
  <div class="timer-wrapper">
    <TimerCard />
  </div>
</template>

<style lang="scss" scoped>
.timer-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  background-image: url('@/assets/timer-background.png');
  background-repeat: no-repeat;
  background-size: cover;
  width: 100%;
  min-height: 100vh;
}
</style>
