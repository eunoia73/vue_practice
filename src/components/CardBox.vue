<template>
  <div class="card">
    <header class="card-header">
      <slot name="title">기본 제목</slot>
    </header>

    <section class="card-body">
      <slot> 기본내용입니다. </slot>
    </section>

    <footer class="card-footer">
      <slot name="actions"></slot>
    </footer>
  </div>
</template>

<style scoped>
.card {
  width: 320px;
  border-radius: 16px;
  background: white;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  overflow: hidden;
}
.card-header {
  padding: 14px 18px;
  font-weight: bold;
  font-size: 17px;
  background: linear-gradient(135deg, #6366f1, #22c55e);
  color: black;
}
.card-body {
  padding: 18px;
  font-size: 14px;
  color: #334155;
  line-height: 1.6;
}
.card-footer {
  padding: 14px 18px;
  display: flex;
  justify-content: flex-end;
  gap: 8px;
  background: #f8fafc;
}
</style>
