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
        <div class="modal-title">Setting</div>

        <button @click="onClose" class="close-btn">
          <img src="@/assets/close-icon.svg" alt="Close icon">
        </button>
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
  padding: 25px;
  .modal-backdrop {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba($color: #000000, $alpha: 0.5);
  }
  .modal-content {
    position: relative;
    background: #092143;
    border-radius: 20px;
    margin: auto;
    max-width: 500px;
    color: #ffffff !important;
    .modal-header {
      position: relative;
      padding: 12.5px 25px 0 25px;
      .modal-title {
        text-align: center;
        font-size: 24px;
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
