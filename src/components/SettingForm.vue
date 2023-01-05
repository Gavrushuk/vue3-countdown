<script lang="ts" setup>
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import { computed, ref } from 'vue';

const emit = defineEmits<{
  (e: 'onSave', data: any): void
}>();

const props = defineProps<{
  defaultTitle: string,
  defaultDate: Date
}>();

const title = ref(props.defaultTitle);
const date = ref(props.defaultDate);

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
    date: date.value
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
        date: `December 25, ${year} 00:00:00`
      });
      break;
    case 'new year':
      emit('onSave', {
        title: 'Happy New Year',
        date: `Januar 1, ${+currentYear + 1} 00:00:00`
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

    <button @click="onSave" class="save-btn" :disabled="!!isTitleInvalid || !!isDateInvalid">Save</button>

    <div>OR use presets</div>

    <div class="presets">
      <button @click="onPreset('christmas')" class="save-btn">Christmas</button>
      <button @click="onPreset('new year')" class="save-btn">New year</button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.setting-form {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 15px;

  .label {
    margin-bottom: 5px;
  }

  input {
    background-color: #080F1A;
    color: #ffffff;
    height: 38px;
    width: 100%;
    padding: 6px 12px;
    border: 1px solid #080F1A;
    border-radius: 4px;

    &:focus,
    &:active,
    &:hover {
      border-color: #080F1A !important;
      outline: none !important;
    }
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
    height: 38px;
    margin: 0 auto;
    padding: 6px 12px;
    border-radius: 4px;
    font-weight: bold;
    background-color: #A5DEFF;
    border: none;
    cursor: pointer;
  }

  .presets {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-top: 15px;
    .save-btn {
      margin: 0;
    }
  }

  &:deep(.dp__input) {
    background-color: #080F1A;
    border-color: #080F1A;
    color: #ffffff;

    &~svg {
      path {
        fill: #ffffff;
      }
    }
  }
}
</style>
