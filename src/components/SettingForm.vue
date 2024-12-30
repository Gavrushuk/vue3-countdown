<script lang="ts" setup>
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import { computed, ref } from 'vue';

const emit = defineEmits<{
  (e: 'onSave', data: { title: string, date: Date | string, bgImgUrl: string }): void
}>();

const props = defineProps<{
  defaultTitle: string,
  defaultDate: Date,
  defaultBgImgUrl: string,
}>();

const title = ref(props.defaultTitle);
const date = ref(props.defaultDate);
const bgImgUrl = ref(props.defaultBgImgUrl)

const isTitleInvalid = computed(() => {
  if (!title.value.trim()) {
    return 'Title is required';
  }

  return '';
});

const isDateInvalid = computed(() => {
  const currentDateTime = new Date();
  const selectedDateTime = new Date(date.value);

  if (!date.value) {
    return 'Date and Time is required';
  }

  if (currentDateTime.getTime() > selectedDateTime.getTime()) {
    return 'Date and Time can not less then current';
  }

  return '';
});

const onSave = () => {
  emit('onSave', {
    title: title.value.trim(),
    date: date.value,
    bgImgUrl: bgImgUrl.value.trim()
  });
};

const onPreset = (type: string) => {
  const currentMinutes = new Date().getMinutes();
  const currentHour = new Date().getHours();
  const currentDay = new Date().getDate();
  const currentMonth = new Date().getMonth() + 1;
  const currentYear = new Date().getFullYear();

  switch (type) {
    case 'christmas':
      let year = currentYear;

      if (+currentMinutes >= 0 && +currentHour >= 0 && +currentDay >= 25 && +currentMonth >= 12) {
        year = +currentYear + 1;
      }

      emit('onSave', {
        title: 'Merry Christmas',
        date: `December 25, ${year} 00:00:00`,
        bgImgUrl: '',
      });
      break;
    case 'new year':
      emit('onSave', {
        title: 'Happy New Year',
        date: `Januar 1, ${+currentYear + 1} 00:00:00`,
        bgImgUrl: '',
      });
      break;
  }
};
</script>

<template>
  <div class="setting-form">
    <div>
      <div class="label">Title *:</div>
      <input v-model="title" type="text" maxlength="40">
      <div class="error" :class="{ active: !!isTitleInvalid }">* {{ isTitleInvalid }}</div>
    </div>

    <div>
      <div class="label">Date and Time *:</div>
      <Datepicker v-model="date" :min-date="new Date()" :clearable="false"></Datepicker>
      <div class="error" :class="{ active: !!isDateInvalid }">* {{ isDateInvalid }}</div>
    </div>

    <div>
      <div class="label">Background Image:</div>
      <input v-model="bgImgUrl" type="string">
    </div>

    <button @click="onSave" class="save-btn" :disabled="!!isTitleInvalid || !!isDateInvalid">Save</button>

    <div>Select Date Preset</div>

    <div class="presets">
      <button @click="onPreset('christmas')" class="preset">Merry Christmas</button>
      <button @click="onPreset('new year')" class="preset">Happy New Year</button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.setting-form {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 15px;
  color: #000000;

  .label {
    margin-bottom: 5px;
    color: #000000;
  }

  input {
    background-color: #FFFFFF;
    color: #000000;
    height: 38px;
    font-size: 16px;
    line-height: 24px;
    font-weight: 400;
    width: 100%;
    padding: 6px 12px;
    border: 1px solid transparent;
    border-radius: 16px;
  }

  .error {
    display: none;
    color: red;
    margin-top: 5px;

    &.active {
      display: block;
    }
  }

  .save-btn {
    display: inline-flex;
    align-self: flex-start;
    height: 40px;
    padding: 11px 28px;
    border-radius: 50px;
    font-weight: bold;
    background-color: #000000;
    color: #ffffff;
    border: none;
    cursor: pointer;
  }

  .presets {
    display: flex;
    gap: 10px;
  }

  .preset {
    background-color: #FFFFFF;
    border-radius: 16px;
    padding: 24px;
    flex: 1;
    color: #000000;
    border: none;
    cursor: pointer;
  }

  &:deep(.dp__input) {
    background-color: #FFFFFF;
    color: #000000;
    border-color: transparent;
    border-radius: 16px;

    &~svg {
      path {
        fill: #000000;
      }
    }
  }
}
</style>
