<template>
  <div>
    <div class="title">Laporan</div>
    <div class="table">
      <table>
        <caption>Laporan Omset</caption>
        <thead>
          <tr>
            <th>#</th>
            <th>Tanggal Transaksi</th>
            <th>Total Pembayaran</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="pending">
            <td colspan="3"><Loader class="loader" /></td>
          </tr>
          <tr v-for="(transaction, index) in transaksi" :key="transaction.id">
            <th>{{ index + 1 }}</th>
            <td>{{ transaction.tgl_transaksi }}</td>
            <td>{{ transaction.total_bayar }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="container">
      <Bar :data="chartData" />
    </div>
  </div>
</template>

<script setup>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

definePageMeta({
  layout: 'main',
  middleware: 'auth'
})

const supabase = useSupabaseClient()

const { data: transaksi, pending } = useAsyncData('transaksi', async () => {
  const { data, error } = await supabase.from('transaksi').select()
  if (error) throw error
  return data
})

const chartData = computed(() => {
  return {
    labels: ['30-04-2024'],
    datasets: [{
      label: 'Omset',
      data: [449000],
      backgroundColor: '#7373ff'
    }]
  }
}) 
</script>

<style scoped>
@import url('~/assets/css/main.css');
</style>