<template>
  <div>
    <button class="btn">
      <slot>버튼</slot>
    </button>
  </div>
</template>

<style scoped>
.btn {
  padding: 10px 18px;
  border-radius: 10px;
  border: none;
  background: linear-gradient(135deg, #6366f1 #22c55e);
  color: white;
  font-size: 15px;
  cursor: pointer;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}
.btn:hover {
  opacity: 0.9;
}
</style>
