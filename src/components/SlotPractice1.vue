<template>
  <div class="page">
    <BaseButton>저장</BaseButton>
    <BaseButton>삭제</BaseButton>
    <BaseButton> 알림 보내기 </BaseButton>
  </div>
</template>

<script setup>
import BaseButton from './BaseButton.vue'
</script>

<style scoped>
.page {
  height: 100vh;
  display: flex;
  gap: 12px;
  justify-content: center;
  align-items: center;
  background: #f1f5f9;
}
</style>
