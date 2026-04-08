<template>
  <div class="shopping-cart">
    <h2>🛒 쇼핑 카트</h2>

    <!-- 상품 목록 -->
    <div class="products">
      <h3>상품 목록</h3>
      <div v-for="product in products" :key="product.id" class="product-item">
        <div class="product-info">
          <strong>{{ product.name }}</strong>
          <span class="price">{{ product.price.toLocaleString() }}원</span>
        </div>
        <div class="quantity-control">
          <button @click="decreaseQuantity(product.id)">-</button>
          <span class="quantity">{{ product.quantity }}</span>
          <button @click="increaseQuantity(product.id)">+</button>
        </div>
      </div>
    </div>
    <!-- 계산 결과(Computed) -->
    <div class="summary">
      <h3>주문요약</h3>
      <div class="summary-item">
        <span>총 상품 수:</span>
        <strong>{{ totalItems }}개</strong>
      </div>
      <div class="summary-item">
        <span>상품 금액:</span>
        <strong>{{ subtotal.toLocaleString() }}원</strong>
      </div>
      <div class="summary-item">
        <span>배송비:</span>
        <strong :class="{ free: isFreeShipping }">
          {{ shippingCost.toLocaleString() }}원
          <span v-if="isFreeShipping" class="badge">무료</span>
        </strong>
      </div>
      <div class="summary-item">
        <span>할인:</span>
        <strong class="discount">-{{ discount.toLocaleString() }}원</strong>
      </div>
      <div class="summary-item total">
        <span>최종 금액:</span>
        <strong class="total-price">{{ finalTotal.toLocaleString() }}원</strong>
        <p v-if="!isFreeShipping" class="free-shipping-message">
          {{ freeShippingRemaining.toLocaleString() }}원 더 구매하시면 무료 배송!
        </p>
        <button class="checkout-btn">결제하기</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'

const products = ref([
  { id: 1, name: '노트북', price: 1500000, quantity: 1 },
  { id: 2, name: '마우스', price: 30000, quantity: 0 },
  { id: 3, name: '키보드', price: 80000, quantity: 0 },
])

// 수량 추가
const increaseQuantity = (id) => {
  const product = products.value.find((p) => p.id === id)
  if (product) {
    product.quantity++
  }
}
// 수량 감소
const decreaseQuantity = (id) => {
  const product = products.value.find((p) => p.id === id)
  if (product) {
    product.quantity--
  }
}

// Computed 1 : 총 상품 수
const totalItems = computed(() => {
  // computed -> products가 바뀌면 자동으로 다시 계산됨
  return products.value.reduce((sum, product) => {
    return sum + product.quantity
  }, 0)
})
/**
 * array.reduce((누적값, 현재값) => {
 * return 새로운 누적값
 * }, 초기값)
 */

// Computed 2 : 상품 금액 합계
const subtotal = computed(() => {
  return products.value.reduce((sum, product) => {
    return sum + product.price * product.quantity
  }, 0)
})

// Computed 3 : 무료 배송 여부(5만원 이상)
const isFreeShipping = computed(() => {
  return subtotal.value >= 50000
})

// Computed 4 : 배송비
const shippingCost = computed(() => {
  return isFreeShipping.value ? 0 : 3000
})

// Computed 5 : 무료 배송까지 남은 금액
const freeShippingRemaining = computed(() => {
  return Math.max(0, 50000 - subtotal.value)
})

// Computed 6 : 할인 금액(10만원 이상 구매시 5% 할인)
const discount = computed(() => {
  return subtotal.value >= 1000000 ? Math.floor(subtotal.value * 0.05) : 0
})

// Computed 7 : 최종 금액
const finalTotal = computed(() => {
  return subtotal.value + shippingCost.value - discount.value
})
</script>

<style scoped>
.shopping-cart {
  max-width: 800px;
  margin: 20px auto;
  padding: 20px;
}
h2 {
  color: #2c3e50;
  text-align: center;
  margin-bottom: 30px;
}
h3 {
  color: #2c3e50;
  margin-top: 0;
  margin-bottom: 20px;
}
.products {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}
.product-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border-bottom: 1px solid #eee;
}
.product-item:last-child {
  border-bottom: none;
}
.product-info {
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.product-info strong {
  font-size: 16px;
  color: #2c3e50;
}
.price {
  color: #2c3e50;
  font-size: 14px;
}
.quantity-control {
  display: flex;
  align-items: center;
  gap: 15px;
}
.quantity-control button {
  width: 32px;
  height: 32px;
  border: 2px solid #3498db;
  background: white;
  color: #3498db;
  border-radius: 4px;
  cursor: pointer;
  font-size: 18px;
  font-weight: bold;
}
.quantity-control button :hover {
  background: #3498db;
  color: white;
}
.quantity {
  /* min-width: 30px; */
  text-align: center;
  font-weight: 600;
  font-size: 16px;
  color: #7f8c8d;
}
.summary {
  background: white;
  padding: 25px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
.summary-item {
  display: flex;
  justify-content: space-between;
  padding: 12px 0;
  border-bottom: 1px solid #eee;
}
.summary-item.total {
  border-top: 2px solid #2c3e50;
  border-bottom: none;
  margin-top: 10px;
  padding-top: 15px;
}
.summary-item span {
  color: #7f8c8d;
}
.summary-item strong {
  color: #2c3e50;
  font-size: 16px;
}
.discount {
  color: #e74c3c !important;
}
.total-price {
  font-size: 24px !important;
  color: #3498db !important;
}
.free {
  color: #27ae60 !important;
}
.badge {
  background: #27ae60;
  color: white;
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 12px;
  margin-left: 5px;
}
.free-shipping-message {
  margin-top: 15px;
  padding: 12px;
  background: #e8f4f8;
  border-left: 4px solid #3498db;
  border-radius: 4px;
  color: #2c3e50;
  font-size: 14px;
}
.checkout-btn {
  width: 100%;
  padding: 15px;
  margin-top: 20px;
  background: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
}
.checkout-btn:hover {
  background: #2980b9;
}
</style>
