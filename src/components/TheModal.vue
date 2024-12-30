<script lang="ts" setup>
const emit = defineEmits<{
  (e: 'onClose'): void
}>();

const onClose = () => {
  emit('onClose');
};
</script>

<template>
  <div class="modal">
    <div @click="onClose" class="modal-backdrop"></div>
    <div class="modal-content">
      <div class="modal-header">
        <div class="modal-title">Set up</div>
      </div>
      <div class="modal-body">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.modal {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  padding: 24px;
  .modal-backdrop {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: transparent;
  }
  .modal-content {
    position: relative;
    top: 50%;
    background: rgba($color: #ffffff, $alpha: 0.8);
    border-radius: 28px;
    margin: auto;
    max-width: 500px;
    color: #ffffff !important;
    transform: translateY(-50%);
    box-shadow: 0px 38px 90px 0px rgba(0, 0, 0, 0.25);
    backdrop-filter: blur(80px);
    .modal-header {
      position: relative;
      padding: 24px 24px 0 24px;
      .modal-title {
        font-size: 24px;
        color: #343946;
      }
      .close-btn {
        position: absolute;
        top: 12.5px;
        right: 12.5px;
        border: none;
        background: transparent;
        cursor: pointer;
        img {
          width: 25px;
          height: 25px;
        }
      }
    }
    .modal-body {
      padding: 25px;
    }
  }
}
</style>
