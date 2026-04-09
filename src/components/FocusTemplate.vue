<template>
  <div class="page">
    <div class="row" v-for="(item, index) in items" :key="item.id">
      <label>{{ item.label }}</label>

      <input ref="inputRefs" type="text" placeholder="입력하세요" />
      <button @click="focusInput(index)">포커스</button>
    </div>
  </div>
</template>

<script setup>
import { nextTick, ref } from 'vue'

const items = ref([
  { id: 1, label: '이름' },
  { id: 2, label: '이메일' },
  { id: 3, label: '전화번호' },
])

// v-for + ref -> 배열
const inputRefs = ref([])

function focusInput(index) {
  nextTick(() => {
    inputRefs.value[index].focus()
  })
}
</script>

<style scoped>
.page {
  padding: 30px;
}
.row {
  display: flex;
  gap: 8px;
  align-items: center;
  margin-bottom: 12px;
}
input {
  padding: 6px 8px;
}
</style>
