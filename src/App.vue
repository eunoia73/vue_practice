<script setup lang="ts">
import ShoppingCart from './components/ShoppingCart.vue'
import ShoppingCart2 from './components/ShoppingCart2.vue'
import SlotPractice1 from './components/SlotPractice1.vue'
import SlotPractice2 from './components/SlotPractice1.vue'
import UserProfile from './components/UserProfile.vue'
</script>

<template>
  <div>
    <!-- 쇼핑카트 예제(computed) -->
    <!-- <ShoppingCart /> -->
    <!-- 사용자 정보 변경 예제(watch) -->
    <!-- <UserProfile /> -->
    <!-- 쇼핑몰 장바구니 실시간 할인 예제(watchEffect) -->
    <!-- <ShoppingCart2 /> -->
    <!-- slot 예제 1 -->
    <SlotPractice1 />
    <!-- slot 예제 2(slot name 지정) -->
    <SlotPractice2 />
  </div>
</template>
