<script setup lang="ts">
import TimerCard from '@/components/TimerCard.vue';
import TheModal from "@/components/TheModal.vue";
import SettingForm from "@/components/SettingForm.vue";
import { onMounted, ref } from 'vue';

const isOpenedSettingModal = ref(false);
const title = ref('Happy New Year');
const date = ref(new Date('Januar 1, 2023 00:00:00'));

onMounted(() => {
  syncSetting();
});

const syncSetting = () => {
  try {
    if (localStorage.setting) {
      title.value = JSON.parse(localStorage.setting).title;
      date.value = new Date(JSON.parse(localStorage.setting).date);
    }
  } catch (error) {
    console.error(error);
    title.value = 'Happy New Year';
    date.value = new Date('Januar 1, 2023 00:00:00');
  }
}

const updateSetting = () => {
  localStorage.setItem('setting', JSON.stringify({ title: title.value, date: date.value }));
  syncSetting();
}

const onSave = (data: any) => {
  title.value = data.title;
  date.value = new Date(data.date);
  isOpenedSettingModal.value = false;
  updateSetting();
};
</script>

<template>
  <div class="timer-wrapper">
    <button @click="isOpenedSettingModal = true" class="setting-btn">
      <img src="@/assets/setting-icon.svg" alt="Setting icon">
    </button>

    <TimerCard
      :title="title"
      :date="date"
    />

    <TheModal
      v-if="isOpenedSettingModal"
      @on-close="isOpenedSettingModal = false"
    >
      <SettingForm
        :defaultTitle="title"
        :defaultDate="date"
        @on-save="onSave"
      />
    </TheModal>
  </div>
</template>

<style lang="scss" scoped>
.timer-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  background-image: url('@/assets/timer-background.png');
  background-repeat: no-repeat;
  background-size: cover;
  width: 100%;
  min-height: 100vh;
  padding: 25px;
  .setting-btn {
    position: absolute;
    top: 25px;
    right: 25px;
    background: transparent;
    border: none;
    cursor: pointer;
    img {
      width: 40px;
      height: 40px;
    }
  }
}
</style>
