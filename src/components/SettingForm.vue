<script lang="ts" setup>
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';
import { ref } from 'vue';

const emit = defineEmits<{
  (e: 'onSave', data: any): void
}>();

const props = defineProps<{
  defaultTitle: string,
  defaultDate: Date
}>();

const title = ref(props.defaultTitle);
const date = ref(props.defaultDate);

const onSave = () => {
  emit('onSave', {
    title: title.value,
    date: date.value
  });
};
</script>

<template>
  <div class="setting-form">
    <div>
      <div class="label">Title:</div>
      <input v-model="title" type="text">
    </div>
    <div>
      <div class="label">Date and Time:</div>
      <Datepicker v-model="date"></Datepicker>
    </div>
    <button @click="onSave" class="save-btn">Save</button>
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
