<template>
  <div>
    <div class="title"><NuxtLink to="/obat">Obat</NuxtLink> > Tambah</div>
    <form @submit.prevent="insertObat">
      <div class="input-group">
        <div class="input">
          <input type="text" v-model="form.kode_obat" placeholder=" " required>
          <label>Kode Obat</label>
        </div>
        <div class="input">
          <input type="text" v-model="form.nama_obat" placeholder=" " required>
          <label>Nama Obat</label>
        </div>
        <div class="input">
          <input type="date" v-model="form.expired_date" placeholder=" " required>
          <label>Expired Date</label>
        </div>
      </div>
      <div class="input-group">
        <div class="input">
          <input type="number" v-model="form.jumlah" placeholder=" " required>
          <label>Jumlah</label>
        </div>
        <div class="input">
          <input type="text" v-model="form.harga" placeholder=" " required>
          <label>Harga</label>
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
  kode_obat: '',
  nama_obat: '',
  expired_date: '',
  jumlah: '',
  harga: '',
})

const { execute: insertObat, status, error } = useAsyncData('obat', async () => {
  const { error } = await supabase.from('obat').insert(form.value)
  if (error) throw error
  else navigateTo('/obat')
}, { immediate: false })
</script>

<style scoped>
@import url('~/assets/css/main.css');
</style>