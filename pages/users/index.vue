<template>
  <div>
    <div class="title">Users</div>
    <div class="table">
      <table>
        <caption>Users</caption>
        <thead>
          <tr>
            <th>#</th>
            <th>Tipe User</th>
            <th>Nama</th>
            <th>Alamat</th>
            <th>Telpon</th>
            <th>Username</th>
            <th>Email</th>
            <th>Password</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="pending">
            <td colspan="9"><Loader class="loader" /></td>
          </tr>
          <tr v-else v-for="(user, index) in users" :key="user.id">
            <td>{{ index + 1 }}</td>
            <td>{{ user.tipe_user }}</td>
            <td>{{ user.nama_user }}</td>
            <td>{{ user.alamat }}</td>
            <td>{{ user.telpon }}</td>
            <td>{{ user.username }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.password }}</td>
            <td>
              <div class="btn-icon" @click="navigateTo(`/users/${user.id}`)">
                <img src="~/assets/img/edit.png" alt="Edit">
              </div>
              <div class="btn-icon" @click="confirmDelete = true">
                <img src="~/assets/img/delete.png" alt="Delete">
              </div>
              <DeleteConfirmation v-if="confirmDelete" @cancel="confirmDelete = false" @delete="deleteUser(user.id)" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="container">
      <button class="submit" @click="navigateTo('/users/tambah')">Tambah User</button>
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

const { data: users, pending, refresh } = useAsyncData('users', async () => {
  const { data, error } = await supabase.from('users').select()
  if (error) throw error
  return data
})

const confirmDelete = ref(false)

async function deleteUser(id) {
  const data = await $fetch(`/api/user/${id}`, {
    method: 'DELETE'
  })
  if (data) {
    confirmDelete.value = false
    refresh()
  }
}
</script>

<style scoped>
@import url('~/assets/css/main.css');
</style>