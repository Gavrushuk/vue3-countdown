<script setup lang="ts">
import TimerCard from '@/components/TimerCard.vue';
import TheModal from "@/components/TheModal.vue";
import SettingForm from "@/components/SettingForm.vue";
import { onMounted, ref } from 'vue';
import defaultBgImg from '@/assets/countdown-bg.webp';

const isOpenedSettingModal = ref(false);
const title = ref('Happy New Year');
const date = ref(new Date(`Januar 1, ${+(new Date().getFullYear()) + 1} 00:00:00`));
const bgImgUrl = ref('');

onMounted(() => {
  syncSetting();
});

const syncSetting = () => {
  try {
    if (localStorage.setting) {
      title.value = JSON.parse(localStorage.setting).title;
      date.value = new Date(JSON.parse(localStorage.setting).date);
      bgImgUrl.value = JSON.parse(localStorage.setting)?.bgImgUrl || "";
    }
  } catch (error) {
    console.log(error);
  }
}

const updateSetting = () => {
  localStorage.setItem('setting', JSON.stringify({ title: title.value, date: date.value, bgImgUrl: bgImgUrl.value }));
  syncSetting();
}

const onSave = (data: any) => {
  title.value = data.title;
  date.value = new Date(data.date);
  bgImgUrl.value = data.bgImgUrl;
  isOpenedSettingModal.value = false;
  updateSetting();
};
</script>

<template>
  <div class="timer-wrapper">
    <div class="timer-content" :style="{backgroundImage: `url('${bgImgUrl || defaultBgImg}'`}">
      <TimerCard :title="title" :date="date" @click="isOpenedSettingModal = true" />

      <TheModal v-if="isOpenedSettingModal" @on-close="isOpenedSettingModal = false">
        <SettingForm :defaultTitle="title" :defaultDate="date" :defaultBgImgUrl="bgImgUrl" @on-save="onSave" />
      </TheModal>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.timer-wrapper {
  background-color: #000000;
  padding: 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;

  .timer-content {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    background-repeat: no-repeat;
    background-size: cover;
    width: 100%;
    padding: 25px;
    border-radius: 13px;
    flex: 1;

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
}
</style>
