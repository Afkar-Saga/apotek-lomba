<template>
  <div class="login-container">
    <div class="decor"></div>
    <form @submit.prevent="login" class="main">
      <h1>Apotek XYZ</h1>
      <img src="~/assets/img/logo_apotek.png" alt="Logo Apotek">
      <div v-if="status == 'error'">{{ error.message }}</div>
      <div class="input">
        <input type="text" v-model="username" id="username" @input="checkUsername" placeholder=" " required>
        <label>Username</label>
      </div>
      <div class="input">
        <input type="password" v-model="password" id="password" placeholder=" " required>
        <label>Password</label>
      </div>
      <Loader v-if="status == 'pending'" />
      <input v-else type="submit" value="Login" class="submit">
    </form>
    <div class="decor"></div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const username = ref('')
const password = ref('')

const { data: users } = useAsyncData('users', async () => {
  const { data, error } = await supabase.from('users').select()
  if (error) throw error
  return data
})

const checkUsername = () => {
  users.value.forEach(user => {
    if (user.username == username.value) {
      document.getElementById('password').focus()
    }
  })
}

const { execute: login, status, error } = useAsyncData('login', async () => {
  try {
    const { data: user, error } = await supabase.from('users').select('id, email').eq('username', username.value).maybeSingle()
    if (error) throw error
    if (user) {
      const { error } = await supabase.auth.signInWithPassword({
        email: user.email,
        password: password.value
      })
      if (error) throw error
      else {
        const { data, error } = await supabase.from('log').insert({
          aktivitas: 'Login',
          id_user: user.id
        })
        if (error) throw error
        else navigateTo('/')
      }
    }
  } catch (error) {
    console.error(error.message)
  }
}, { immediate: false })

onMounted(() => {
  document.getElementById('username').focus()
})
</script>

<style scoped>
@import url('~/assets/css/main.css');
.login-container {
  width: 1000px;
  height: 100vh;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.decor {
  width: 200px;
  height: 100%;
  background-color: #090;
}
h1 {
  font-size: 2.5em;
}
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 30px;
}
</style>