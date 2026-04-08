<template>
  <div class="shopping-cart">
    <div class="cart-container">
      <!-- 헤더 -->
      <div class="cart-header">
        <h1>🛍️ 장바구니</h1>
        <span class="item-count">{{ cart.length }}개 상품</span>
      </div>

      <!-- 상품 리스트 -->
      <div class="product-list">
        <div v-for="item in cart" :key="item.id" class="product-item">
          <img :src="item.image" alt="item.name" />
          <div class="product-info">
            <h3>{{ item.name }}</h3>
            <p class="price">{{ item.price.toLocaleString() }}원</p>
          </div>
          <div class="quantity-box">
            <button @click="item.quantity--" :disabled="item.quantity <= 1">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="item.quantity++">+</button>
          </div>
        </div>
      </div>

      <!-- 쿠폰 선택 -->
      <div class="coupon-section">
        <label>💵 쿠폰 선택</label>
        <select v-model="selectedCoupon">
          <option value="">쿠폰 없음</option>
          <option value="5">5% 할인</option>
          <option value="10">10% 할인</option>
          <option value="15">15% 할인</option>
        </select>
      </div>

      <!-- 실시간 금액 요약 -->
      <div class="summary-box">
        <div class="summary-row">
          <span>상품 금액</span>
          <span>{{ subtotal.toLocaleString() }}원</span>
        </div>

        <div class="summary-row discount" v-if="discount > 0">
          <span>쿠폰 할인 ({{ selectedCoupon }}%)</span>
          <span>-{{ discount.toLocaleString() }}원</span>
        </div>

        <div class="summary-row">
          <span>배송비</span>
          <span :class="{ free: isFreeShipping }">
            {{ shippingFee.toLocaleString() }}원
            <span v-if="isFreeShipping" class="badge">무료</span>
          </span>
        </div>

        <div v-if="!isFreeShipping && freeShippingGap > 0" class="free-shipping-notice">
          {{ freeShippingGap }}원 더 담으면 무료배송!
        </div>

        <div class="summary-row total">
          <span>최종 결제 금액</span>
          <span class="total-price">{{ totalPrice.toLocaleString() }}원</span>
        </div>

        <div class="savings-info" v-if="discount > 0">{{ discount.toLocaleString() }}원 절약!</div>
      </div>

      <!-- 주문하기 버튼 -->
      <button class="checkout-btn">{{ totalPrice.toLocaleString() }}원 결제하기</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watchEffect } from 'vue'

// 장바구니 상품
const cart = ref([
  {
    id: 1,
    name: '에어팟 프로',
    price: 329000,
    quantity: 1,
    image: 'https://images.unsplash.com/photo-1606841837239-c5a1a4a07af7?w=100&h=100&fit=crop',
  },
  {
    id: 2,
    name: '아이폰 케이스',
    price: 25000,
    quantity: 2,
    image: 'https://images.unsplash.com/photo-1601784551446-20c9e07cdbdb?w=100&h=100&fit=crop',
  },
])

const selectedCoupon = ref('')

// 상품 금액 합계
const subtotal = computed(() => {
  return cart.value.reduce((sum, item) => {
    return sum + item.price * item.quantity
  }, 0)
})

// 쿠폰 할인 금액
const discount = computed(() => {
  if (!selectedCoupon.value) return 0
  return Math.floor(subtotal.value * (selectedCoupon.value / 100))
})

// 무료 배송 여부(5만원 이상)
const isFreeShipping = computed(() => subtotal.value >= 50000)

// 배송비
const shippingFee = computed(() => (isFreeShipping.value ? 0 : 3000))

// 무료 배송까지 남은 금액
const freeShippingGap = computed(() => {
  return Math.max(0, 50000 - subtotal.value)
})

// 최종 결제 금액
const totalPrice = computed(() => {
  return subtotal.value - discount.value + shippingFee.value
})

// watchEffect: 장비구니 변경 시 자동 실행
watchEffect(() => {
  console.log('📊 장바구니 업데이트!')
  console.log('상품 금액:', subtotal.value)
  console.log('할인 금액:', discount.value)
  console.log('배송비:', shippingFee.value)
  console.log('최종 금액:', totalPrice.value)
  // LocalStorage에 자동 저장
  localStorage.setItem('cart', JSON.stringify(cart.value))
  localStorage.setItem('coupon', selectedCoupon.value)
})

// watchEffect: 페이지 로드 시 저장된 데이터 불러오기
watchEffect(() => {
  const savedCart = localStorage.getItem('cart')
  const savedCoupon = localStorage.getItem('coupon')

  if (savedCart) {
    try {
      const parsed = JSON.parse(savedCart)
      if (parsed.length > 0) {
        cart.value = parsed
      }
    } catch (e) {
      console.log('장바구니 로드 실패')
    }
  }
  if (savedCoupon) {
    selectedCoupon.value = savedCoupon
  }
})
</script>

<style scoped>
.shopping-cart {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 40px 20px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}
.cart-container {
  max-width: 500px;
  margin: 0 auto;
  background: white;
  border-radius: 20px;
  padding: 30px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
}

/* 헤더 */
.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
  padding-bottom: 20px;
  border-bottom: 2px solid #f0f0f0;
}

.cart-header h1 {
  margin: 0;
  font-size: 28px;
  color: #2c3e50;
}
.item-count {
  background: #667eea;
  color: white;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
}

/* 상품 리스트 */
.product-list {
  margin-bottom: 25px;
}
.product-item {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px;
  background: #f8f9fa;
  color: #2c3e50;
  border-radius: 12px;
  margin-bottom: 12px;
  transition: all 0.2s;
}
.product-item:hover {
  background: #e9ecef;
  transform: translateY(-2px);
}
.product-item img {
  width: 70px;
  height: 70px;
  border-radius: 10px;
  object-fit: cover;
}

/* 수량 조절 */
.quantity-box {
  display: flex;
  align-items: center;
  gap: 12px;
  background: white;
  padding: 6px 10px;
  border-radius: 8px;
}

.quantity-box button {
  width: 28px;
  height: 28px;
  border: none;
  background: #667eea;
  color: white;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s;
}

.quantity-box button:hover:not(:disabled) {
  background: #5568d3;
  transform: scale(1.1);
}

.quantity-box button:disabled {
  background: #ddd;
  cursor: not-allowed;
}

.quantity-box span {
  min-width: 20px;
  text-align: center;
  font-weight: 600;
  color: #2c3e50;
}

/* 쿠폰 */
.coupon-section {
  margin-bottom: 25px;
  padding: 15px;
  background: #fff9e6;
  border-radius: 12px;
  border: 2px dashed #ffd700;
}

.coupon-section label {
  display: block;
  margin-bottom: 10px;
  font-weight: 600;
  color: #2c3e50;
  font-size: 15px;
}

.coupon-section select {
  width: 100%;
  padding: 12px;
  border: 2px solid #ffd700;
  border-radius: 8px;
  font-size: 14px;
  background: white;
  cursor: pointer;
  font-weight: 500;
}

/* 요약 박스 */
.summary-box {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 12px;
  margin-bottom: 20px;
}

.price {
  margin: 0;
  color: #667eea;
  font-weight: 700;
  font-size: 15px;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 12px;
  font-size: 15px;
  color: #495057;
}

.summary-row.discount {
  color: #e74c3c;
  font-weight: 600;
}

.summary-row.total {
  margin-top: 15px;
  padding-top: 15px;
  border-top: 2px solid #dee2e6;
  font-size: 16px;
  font-weight: 700;
}

.total-price {
  color: #667eea;
  font-size: 24px;
}

.product-info {
  flex: 1;
}

.product-info h3 {
  margin: 0 0 8px 0;
  font-size: 16px;
  color: #2c3e50;
  font-weight: 600;
}

.free {
  color: #27ae60;
  font-weight: 600;
}

.badge {
  background: #27ae60;
  color: white;
  padding: 3px 8px;
  border-radius: 10px;
  font-size: 11px;
  margin-left: 6px;
}

.free-shipping-notice {
  margin: 12px 0;
  padding: 10px;
  background: #e8f5e9;
  border-left: 4px solid #27ae60;
  border-radius: 6px;
  font-size: 13px;
  color: #2c3e50;
  font-weight: 600;
}

.savings-info {
  margin-top: 12px;
  padding: 10px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-align: center;
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
}

/* 결제 버튼 */
.checkout-btn {
  width: 100%;
  padding: 18px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 18px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
}

.checkout-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
}

.checkout-btn:active {
  transform: translateY(0);
}
</style>
