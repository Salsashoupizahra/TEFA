<template>
  <div class="container-fluid ">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">ISI BUKU KUNJUNGAN</h2>
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <input v-model="form.nama" type="text" class="form-control form-control-lg rounded-5" placeholder="Nama...">
          </div>
          <div class="mb-3">
            <select v-model="form.keanggotaan" class="keanggotaan form-control-lg form-select rounded-5">
              <option value="">Keanggotaan</option>
              <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
            </select>
          </div>
          <div v-if="form.keanggotaan == 2" class="mb-3">
            <div class="row">
              <div class="col-md-4">
                <select v-model="form.tingkat" class="tingkat form-control-lg form-select rounded-5 mb-2">
                  <option value="">kelas</option>
                  <option value="X">X</option>
                  <option value="XI">XI</option>
                  <option value="XII">XII</option>
                </select>
              </div>
              <div class="col-md-4">
                <select v-model="form.jurusan" class="jurusan form-control-lg form-select rounded-5 mb-2">
                  <option value="">Jurusan</option>
                  <option value="PPLG">PPLG</option>
                  <option value="TJKT">TJKT</option>
                  <option value="TSM">TSM</option>
                  <option value="TOI">TOI</option>
                  <option value="DKV">DKV</option>
                </select>
              </div>
              <div class="col-md-4">
                <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-5 mb-2">
                  <option value="">tingkat</option>
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                </select>
              </div>
            </div>
          </div>
          <div class="mb-3">
            <select v-model="form.keperluan" class="form-control form-control-lg form-select rounded-5">
              <option value="">Keperluan</option>
              <option v-for="(item,i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
            </select>
          </div>
            <input type="submit" class="btn btn-light btn-lg rounded-5 px-5" placeholder="KIRIM">
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const members = ref([]);
const objectives = ref([]);
const form =ref({
  nama:"",
  keanggotaan:"",
  tingkat:"",
  jurusan:"",
  kelas:"",
  keperluan:"",
});
const kirimData = async() => {
  console.log(form.value)
    const { error } = await supabase.from('pengunjung').insert([form.value]);
    if(!error) navigateTo("/pengunjung/");
};
const getkeanggotaan = async () => {
  const { data,error } = await supabase.from('keanggotaan').select('*');
  if(data) members.value = data;
};
const getkeperluan = async () => {
  const { data, error } = await supabase.from('keperluan').select('*');
  if(data) objectives.value = data;
};
onMounted(() => {
  getkeanggotaan();
  getkeperluan();
});



</script>

<style>
  #wrapper{
    
    width: 650px  ;
    height: auto;
    background-color: rgb(198, 241, 200);
    margin: 0 auto;
    margin-top: 200px;
    border-radius: 10px;
  }
  html, 
  body {
    margin: 0;
    padding: 0;
    background-color:rgb(196, 194, 252);
  }
</style>