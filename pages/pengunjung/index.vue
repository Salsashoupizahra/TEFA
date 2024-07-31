<template>
  <div>
    <div class="container-fluid">
      <div class="row content">
        <div class="col-lg-12">
          <nuxt-link to="/buku">
            <button type="button" class="btn btn-outline-dark mt-4 btn-lg">CARI BUKU</button></nuxt-link>
          <nuxt-link to="/">
            <button type="button" class="btn btn-outline-dark btn-lg mt-4 ms-3"> SELESAI</button></nuxt-link>
            <h2 class="text-center my-4">RIWAYAT PENGUNJUNG</h2>
            </div>
        <nuxt-link to="/">
          <i class="bi bi-caret-left-fill fs-1"></i>
          </nuxt-link>
          <form @submit.prevent="getPengunjung">
            <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5" placeholder="Cari..." @input="getPengunjung" />
          </form>
            <div class="my-3 text-muted"></div>
            <table class="table table-bordered">
              <thead>
                <tr>
                  <td>NO</td>
                  <td>NAMA</td>
                  <td>KATEGORI</td>
                  <td>WAKTU/TANGGAL</td>
                  <td>KEPERLUAN</td>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(visitors,i) in visitor" :key="i">
                  <td>{{ i+1 }}.</td>
                  <td>{{ visitors.nama }}</td>
                  <td>{{ visitors.keanggotaan.nama }}</td>
                  <td>{{ visitors.tanggal }}, {{ visitor.waktu }}</td>
                  <td>{{ visitors.keperluan.nama }}</td>
                </tr>
              </tbody>
            </table>
              
            </div>
          </div>
      </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const keyword = ref('')
const visitor = ref([])

const getPengunjung =async () => {
  const { data, error } = await supabase
  .from('pengunjung')
  .select(`*, keanggotaan(*), keperluan(*)`)
  .ilike("nama",`%${keyword.value}%`)
  if(data) visitor.value = data
}
onMounted(() => {
    getPengunjung()
})
</script>


<style scoped>
@import url('https://saffiwwopurixuwhlsdp.supabase.co');
.content{
  background-color: #D5D5F0;
}
td {
  color:white;
  border:1px solid #FFFFFF;
  background-color: #3F3E67;
}
.container-fluid {
  /* padding: 0;
  margin: 0; */
  background-color: #706F9E(255, 255, 0);
}

h2{
  color:rgb(254, 254, 254)e;
  font-family: "Irish Grover", system-ui;
}
  
.bi-caret-left-fill {
  margin-left: 20px;
}
</style>