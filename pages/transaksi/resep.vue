<template>
  <div>
    <div class="title"><NuxtLink to="/transaksi/resep">Transaksi</NuxtLink> > Resep</div>
    <div class="input">
      <select v-model="selectedResepId" @change="selectResep">
        <option disabled value="0">No Resep</option>
        <option v-for="(resep, index) in reseps" :key="resep.id" :value="resep.id">{{ resep.no_resep }}</option>
      </select>
    </div>
    <div class="container">
      <Loader v-if="status == 'pending'" />
    </div>
    <div v-if="selectedResep && status != 'pending'">
      <div class="container">
        <div class="detail">
          <div>No Resep: {{ selectedResep.no_resep }}</div>
          <div>Tanggal: {{ selectedResep.tgl_resep }}</div>
          <hr>
          <div>Nama Dokter: {{ selectedResep.nama_dokter }}</div>
          <div>Nama Pasien: {{ selectedResep.nama_pasien }}</div>
          <hr>
          <div v-for="(obat, index) in selectedResep.obat" :key="index">
            {{ index + 1 }}. {{ obat.nama_obat }} | Harga: {{ obat.harga }} | Jumlah: {{ selectedResep.resep_obat[index].jumlah_obat }}
          </div>
        </div>
      </div>
      <div class="container">
        <div class="total">Total: Rp. {{ total }}</div>
      </div>
      <div class="container">
        <Loader v-if="bayarStatus == 'pending'" />
        <button v-else class="submit" @click="bayar">Bayar</button>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'main',
  middleware: 'auth'
})

const supabase = useSupabaseClient()
const user = useSupabaseUser()

const selectedResepId = ref(0)
const total = computed(() => {
  return selectedResep.value.obat.reduce((total, current) => {
    return total + (current.harga * current.jumlah_obat)
  }, 0)
})

const { data: reseps } = useAsyncData('reseps', async () => {
  const { data, error } = await supabase.from('resep').select()
  if (error) throw error
  return data
})

const { data: selectedResep, status, execute: selectResep } = useAsyncData('selectedResep', async () => {
  const { data, error } = await supabase.from('resep').select(`
    *,
    obat ( id, nama_obat, harga ),
    resep_obat ( jumlah_obat )
  `).eq('id', selectedResepId.value).maybeSingle()
  if (error) throw error
  data.obat = data.obat.map((obat, index) => ({
    harga: obat.harga,
    jumlah_obat: data.resep_obat[index].jumlah_obat
  }))
  return data
}, { 
  immediate: false,
  watch: selectedResepId.value
})

const { execute: bayar, status: bayarStatus } = useAsyncData('transaksi', async () => {
  try {
    const { error } = await supabase.from('transaksi').insert({
      total_bayar: total.value,
      id_user: user.value.id,
      id_resep: selectedResep.value.id
    })
    if (error) throw error
    else {
      alert('Berhasil melakukan transaksi')
      navigateTo('/transaksi')
    }
  } catch (error) {
    console.error(error.message)
  }
}, { immediate: false })
</script>

<style scoped>
@import url('~/assets/css/main.css');
</style>