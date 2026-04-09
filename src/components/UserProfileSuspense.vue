<template>
  <div class="card">
    <h3>{{ user.name }}</h3>
    <p>{{ user.email }}</p>
  </div>
</template>

<script setup>
const user = await fetch('https://jsonplaceholder.typicode.com/users/1').then((ref) => res.json())
</script>
