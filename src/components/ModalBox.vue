<template>
  <div class="overlay" @click.self="$emit('close')">
    <div class="modal">
      <slot />
    </div>
  </div>
</template>
<script setup></script>
<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal {
  background: white;
  padding: 24px;
  border-radius: 14px;
  min-width: 260px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  color: black;
}
</style>
