<template>
  <div class="page">
    <button class="btn" @click="open">모달 열기</button>

    <ModalBox v-if="isOpen" @close="close">
      <h3>🎉 알림</h3>
      <p>Composable로 제어되는 모달입니다.</p>
      <button class="btn close" @click="close">닫기</button>
    </ModalBox>
  </div>
</template>

<script setup>
import { useModal } from '@/composables/useModal'
import ModalBox from './ModalBox.vue'

const { isOpen, open, close } = useModal()
</script>

<style scoped>
.page {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #f1f5f9;
}
.btn {
  padding: 10px 16px;
  border-radius: 10px;
  border: none;
  background: #6366f1;
  color: white;
  cursor: pointer;
}
.close {
  margin-top: 12px;
  background: #ef4444;
}
</style>
