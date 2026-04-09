<script setup lang="ts">
import UserProfile from './UserProfile.vue'
</script>

<template>
  <Suspense>
    <template #defult>
      <UserProfile />
      <!-- 비동기 컴포넌트 -->
    </template>

    <template #fallback>
      <!-- 로딩 UI -->
      <div class="loading">로딩 중..</div>
    </template>
  </Suspense>
</template>

<style scoped>
.loading {
  padding: 20px;
  font-size: 16px;
}
</style>
