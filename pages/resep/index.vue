<template>
  <div>
    <div class="title">Resep</div>
    <div class="container">
      <div v-if="status == 'error'">{{ error.message }}</div>
      <Loader v-if="pending" />
      <div v-else class="detail" v-for="(resep, index) in reseps" :key="resep.id">
        <div>No Resep: {{ resep.no_resep }}</div>
        <div>Tanggal: {{ resep.tgl_resep }}</div>
        <hr>
        <div>Nama Dokter: {{ resep.nama_dokter }}</div>
        <div>Nama Pasien: {{ resep.nama_pasien }}</div>
        <hr>
        <div v-for="(obat, index) in resep.obat" :key="index">
          {{ index + 1 }}. {{ obat.nama_obat }} | Jumlah: {{ resep.resep_obat[index].jumlah_obat }}
        </div>
        <div class="actions">
          <div class="btn-icon" @click="navigateTo(`/resep/${resep.id}`)">
            <img src="~/assets/img/edit.png" alt="Edit">
          </div>
          <div class="btn-icon" @click="confirmDelete = true">
            <img src="~/assets/img/delete.png" alt="Delete">
          </div>
        </div>
        <DeleteConfirmation v-if="confirmDelete" @cancel="confirmDelete = false" @delete="deleteResep(resep.id)" />
      </div>
    </div>
    <div class="container">
      <button class="submit" @click="navigateTo('/resep/tambah')">Tambah Resep</button>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'main',
  middleware: 'auth'
})

const supabase = useSupabaseClient()

const { data: reseps, pending, status, error, refresh } = useAsyncData('reseps', async () => {
  const { data, error } = await supabase.from('resep').select(`
    *,
    obat ( nama_obat ),
    resep_obat ( jumlah_obat )
  `)
  if (error) throw error
  return data
})

const confirmDelete = ref(false)

async function deleteResep(id) {
  const { error } = await supabase.from('resep').delete().eq('id', id)
  if (error) throw error
  else {
    confirmDelete.value = false
    refresh()
  }
}
</script>

<style scoped>
@import url('~/assets/css/main.css');
.container:has(.detail) {
  justify-content: start;
}
.detail {
  transition: .3s;

  &:hover {
    background-color: #afa;
  }
}
</style>