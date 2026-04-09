<template>
  <div class="page">
    <CardBox>
      <template #title>주문 완료</template>

      주문이 정상적으로 처리되었습니다. 배송 상태는 마이페이지에서 확인할 수 있습니다.

      <template #actions>
        <button class="btn cancel">취소</button>
        <button class="btn confirm">확인</button>
      </template>
    </CardBox>
  </div>
</template>

<script setup>
import CardBox from './CardBox.vue'
</script>

<style scoped>
.page {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #eef2ff;
}
.btn {
  padding: 6px 14px;
  border-radius: 8px;
  border: none;
  font-size: 13px;
  cursor: pointer;
}
.confirm {
  background: #6366f1;
  color: white;
}
.cancel {
  background: #e5e7eb;
}
</style>
