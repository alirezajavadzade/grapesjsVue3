<template>
  <VueFinalModal class="confirm-modal" content-class="confirm-modal-content" overlay-transition="vfm-fade"
    content-transition="vfm-fade">
      <h1>{{ title }}</h1>
    <slot />
  </VueFinalModal>
</template>



<script setup lang="ts">
import { VueFinalModal } from 'vue-final-modal'

defineProps<{
  title?: string
}>()
</script>

<style>
.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.confirm-modal {
  display: flex;
  justify-content: center;
  align-items: center;
}

.confirm-modal-content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 40rem;
  height: 25rem;
  overflow: auto;
  padding: 1rem;
  background: #fff;
  border-radius: 0.5rem;
}

.confirm-modal-content>*+* {
  margin: 0.5rem 0;
}

.confirm-modal-content h1 {
  font-size: 1.375rem;
  text-align: end;
}

.confirm-modal-content button {
  padding: 0.5rem 1rem;
  border-radius: 8px;
  border: 0;
  cursor: pointer;
  max-width: 5rem;
  background: #c9c9c9;
}

.dark .confirm-modal-content {
  background: #000;
}
</style>