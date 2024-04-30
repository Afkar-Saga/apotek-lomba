<template>
  <div>
    <div class="title">Transaksi</div>
    <div class="container">
      <NuxtLink to="/transaksi/resep" class="card-link">Resep</NuxtLink>
      <NuxtLink to="/transaksi/nonresep" class="card-link">Non Resep</NuxtLink>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'main',
  middleware: 'auth'
})
</script>

<style scoped>
@import url('~/assets/css/main.css');
.card-link {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 360px;
  height: 100px;
  margin: 30px;
  background: linear-gradient(to right bottom, #fafafa, #f0f0f0);
  border-radius: 20px;
  text-decoration: none;
  color: black;
  box-shadow: 0 5px 5px rgba(0, 0, 0, 0.25);
  transition: .2s;

  &:hover {
    scale: 1.1;
  }
}
</style>