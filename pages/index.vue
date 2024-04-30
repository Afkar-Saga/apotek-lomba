<template>
  <div>
    <h1>Selamat Datang</h1>
    <h2>{{ nama_user }}</h2>
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'main',
  middleware: 'auth'
})
const supabase = useSupabaseClient()
const user = useSupabaseUser()

const { data: { nama_user } } = await supabase.from('users').select('nama_user').eq('id', user.value.id).maybeSingle()
</script>

<style scoped>
@import url('~/assets/css/main.css');
</style>