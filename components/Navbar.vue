<template>
  <div class="sidenav">
    <div class="brand">
      <img src="~/assets/img/logo_apotek.png" alt="Logo Apotek">
      <span>Apotek XYZ</span>
    </div>
    <nav class="nav">
      <slot></slot>
      <Loader v-if="status == 'pending'" />
      <button v-else type="button" @click="logout">Logout</button>
      <div v-if="status == 'error'">{{ error.message }}</div>
    </nav>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const user = useSupabaseUser()

const { execute: logout, status, error } = useAsyncData('logout', async () => {
  try {
    const { data, error } = await supabase.from('log').insert({
      aktivitas: 'Logout',
      id_user: user.value.id
    })
    if (error) throw error
    else {
      const { error } = await supabase.auth.signOut()
      if (error) throw error
      else navigateTo('/login')
    }
  } catch (error) {
    console.error(error.message)
  }
}, { immediate: false })
</script>

<style scoped>
.sidenav {
  height: 100%;
  background-color: #090;
  color: white;
  width: 100px;
  flex: 0 1 14%;
  box-shadow: 5px 0 3px rgba(0, 0, 0, 0.5);
}
.brand {
  padding: 5px;
  display: flex;
  align-items: center;
  border-bottom: thin solid white;
}
img {
  width: 40px;
  margin-right: 5px;
}
.nav {
  margin: 30px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
}
button {
  background: none;
  border: none;
  color: #ccc;
  cursor: pointer;
  transition: .2s;

  &:hover {
    color: #900;
  }
}
</style>