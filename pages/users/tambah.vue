<template>
  <div>
    <div class="title"><NuxtLink to="/users">Users</NuxtLink> > Tambah</div>
    <form @submit.prevent="insertUser">
      <div class="input-group">
        <div class="input">
          <select v-model="form.tipe_user" :class="{ selected: form.tipe_user }" required>
            <option disabled value="">Tipe User</option>
            <option>Admin</option>
            <option>Apoteker</option>
            <option>Kasir</option>
          </select>
        </div>
        <div class="input">
          <input type="text" v-model="form.nama_user" placeholder=" " required>
          <label>Nama</label>
        </div>
        <div class="input">
          <input type="text" v-model="form.alamat" placeholder=" ">
          <label>Alamat</label>
        </div>
        <div class="input">
          <input type="tel" v-model="form.telpon" placeholder=" ">
          <label>Telpon</label>
        </div>
      </div>
      <div class="input-group">
        <div class="input">
          <input type="text" v-model="form.username" placeholder=" " required>
          <label>Username</label>
        </div>
        <div class="input">
          <input type="text" v-model="form.email" placeholder=" " required>
          <label>Email</label>
        </div>
        <div class="input">
          <input type="text" v-model="form.password" placeholder=" " required>
          <label>Password</label>
        </div>
      </div>
      <div class="container" v-if="status == 'error'">{{ error.message }}</div>
      <div class="container">
        <Loader v-if="status == 'pending'" />
        <input v-else type="submit" class="submit" value="Tambah">
      </div>
    </form>
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'main',
  middleware: 'auth'
})

const supabase = useSupabaseClient()

const form = ref({
  tipe_user: '',
  nama_user: '',
  alamat: '',
  telpon: '',
  username: '',
  email: '',
  password: '',
})

const { execute: insertUser, status, error } = useAsyncData('users', async () => {
  const data = await $fetch('/api/user', {
    method: 'POST',
    body: form.value
  })
  if (data) {
    const { error } = await supabase.from('users').insert({
      id: data.user.id,
      ...form.value
    })
    if (error) throw error
    else navigateTo('/users')
  }
}, { immediate: false })
</script>

<style scoped>
@import url('~/assets/css/main.css');
</style>