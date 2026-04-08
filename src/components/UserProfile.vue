<template>
  <div class="user-profile">
    <h2>사용자 프로필</h2>

    <div class="form">
      <div>
        <label>이름:</label>
        <input v-model="user.profile.name" />
      </div>

      <div>
        <label>나이:</label>
        <input v-model.number="user.profile.age" type="number" />
      </div>

      <div>
        <label>도시:</label>
        <input v-model="user.address.city" />
      </div>

      <div>
        <label>우편번호</label>
        <input v-model="user.address.zipCode" />
      </div>
    </div>

    <div class="log">
      <h3>변경 로그</h3>
      <ul>
        <li v-for="(entry, index) in changeLog" :key="index">
          {{ entry }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
const user = ref({
  profile: {
    name: '홍길동',
    age: 30,
  },
  address: {
    city: '서울',
    zipCode: '12345',
  },
})

const changeLog = ref([])

watch(
  user,
  (newUser) => {
    const timestamp = new Date().toLocaleTimeString()
    changeLog.value.unshift(`[${timestamp}] 사용자 정보 변경 : ${JSON.stringify(newUser, null, 2)}`)

    // 최근 5개만 유지
    if (changeLog.value.length > 5) {
      changeLog.value.pop()
    }
  },
  { deep: true }, // 깊은 감시 user.profile.name, user.address.city 등 모두 감지
)
</script>

<style scoped>
.user-profile {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}
.form > div {
  margin-bottom: 15px;
}
label {
  display: inline-block;
  width: 100px;
  font-weight: 600;
}
input {
  padding: 8px;
  border: 2px solid #ddd;
  border-radius: 4px;
  width: 200px;
}
.log {
  margin-top: 30px;
  padding: 15px;
  background: #f8f9fa;
  border-radius: 4px;
}
.log ul {
  list-style: none;
  padding: 0;
  max-height: 200px;
  overflow-y: auto;
}
.log li {
  padding: 8px;
  margin-bottom: 5px;
  background: white;
  color: #3498db;
  border-left: 3px solid #3498db;
  font-family: monospace;
  font-size: 12px;
}
</style>
