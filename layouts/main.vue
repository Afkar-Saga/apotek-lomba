<template>
  <div>
    <div class="wrapper">
      <Navbar>
        <img src="~/assets/img/logo_admin.png" alt="Admin" v-if="user.user_metadata.tipe_user == 'Admin'">
        <img src="~/assets/img/logo_apoteker.png" alt="Apoteker" v-if="user.user_metadata.tipe_user == 'Apoteker'">
        <img src="~/assets/img/logo_kasir.png" alt="Kasir" v-if="user.user_metadata.tipe_user == 'Kasir'">
        <div class="username">{{ user.user_metadata.username }}</div>
        <NuxtLink to="/log" v-if="['Admin'].includes(user.user_metadata.tipe_user)" :class="{ active: route.name.split('-')[0] == 'log' }">Log Activity</NuxtLink>
        <NuxtLink to="/users" v-if="['Admin'].includes(user.user_metadata.tipe_user)" :class="{ active: route.name.split('-')[0] == 'users' }">Kelola User</NuxtLink>
        <NuxtLink to="/obat" v-if="['Admin', 'Apoteker'].includes(user.user_metadata.tipe_user)" :class="{ active: route.name.split('-')[0] == 'obat' }">Kelola Obat</NuxtLink>
        <NuxtLink to="/resep" v-if="['Admin', 'Apoteker'].includes(user.user_metadata.tipe_user)" :class="{ active: route.name.split('-')[0] == 'resep' }">Kelola Resep</NuxtLink>
        <NuxtLink to="/transaksi" v-if="['Admin', 'Kasir'].includes(user.user_metadata.tipe_user)" :class="{ active: route.name.split('-')[0] == 'transaksi' }">Transaksi</NuxtLink>
        <NuxtLink to="/laporan" v-if="['Admin', 'Kasir'].includes(user.user_metadata.tipe_user)" :class="{ active: route.name.split('-')[0] == 'laporan' }">Laporan</NuxtLink>
      </Navbar>
      <div class="main">
        <main class="content">
          <div class="card">
            <slot />
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script setup>
const route = useRoute()
const user = useSupabaseUser()
</script>

<style scoped>
@import url('~/assets/css/main.css');
div:has(.wrapper) {
  overflow: hidden;
}
.wrapper {
  max-width: 100vw;
  height: 100vh;
  display: flex;
}
.username {
  font-size: 24px;
}
.main {
  width: 100%;
  height: 100%;
  background-color: #fafffa;
  overflow-y: auto;
  flex: 1;
}
.content {
  padding: 3rem;
}
.card {
  width: 100%;
  padding: 18px 32px;
  background-color: white;
  border-radius: 15px;
  box-shadow: 0 0 5px rgba(37, 41, 34, 0.15);
}
img {
  width: 100px;
}
a {
  width: 100%;
  text-align: center;
  padding: 10px 0;
  color: white;
  text-decoration: none;
  transition: .3s;

  &:hover {
    background-color: #070;
  }

  &.active {
    color: #0f0;
    border-left: 3px solid #0c0;
  }
}
</style>