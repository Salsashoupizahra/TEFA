<template>
    <div class="container-fluid">
        <div class="row content">
            <div class="col-lg-12">
                <h2 class="text-center my-4 just" style="color: aliceblue;">RAK BUKU</h2>
                <div class="row d-flex justify-content-center">
                    <div class="col-lg-3">
                        <div class="my-3">
                            <!-- <select class="form-select" aria-label="Default select example"
                                style="box-shadow: 2px px px px px #424242;">
                                <option v-for="kategori in kategories" :key="kategori.id" :value="kategori.nama">{{ kategori.nama }}</option>
                            </select> -->
                        </div>
                    </div>
                    <div class="col-lg-8">
                        <form @submit.prevent="getBooks" class="my-3">
                            <input v-model="keyword" type="search" class="form-control rounded-5 cari" placeholder="SEARCH?..." style="background-color: #D9D9D9;">
                        </form>
                    </div>
                </div>
                <div class="my-3 text-muted"></div>
                <div class="row layer m-5 rounded-5">
                    <h1 class="text-center" style="color: aliceblue;">Koleksi Buku</h1>
                    <div v-for="(book,i) in books" :key="i" class="col-lg-2">
                        <div class="card mb-3">
                            <div class="card-body">
                                <nuxt-link :to="`/buku/${book.id}`">
                                    <img :src="book.cover" class="cover" alt="cover 1" style="width: 100%;">
                                </nuxt-link>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const keyword = ref('')
const books = ref([])
const kategories = ref([])

const getBooks = async () => {
    const { data, error } = await supabase.from('buku').select(`*, kategori_buku(*)`)
    .ilike('judul', `%${keyword.value}%`)
    if(data) books.value = data
    books.value = data; 
          // data.forEach(book => {
          //     const { data } = supabase.storage.from('cover').getPublicUrl(book.cover)
          //     if (data) {
          //         book.cover = data.publicUrl
          //     }
          // })
}


const getKategori = async () => {
    const { data, error } = await supabase.from('kategori_buku').select('*')
    if (data) kategories.value = data
}

onMounted(() => {
    getBooks()
    getKategori()
})
</script>

<style scoped>

.content {
    background-color: #9593D9;
}

/* .cari{
    width: 40rem;
  } */
/* .cover{
    height: 50%;
    width: 50%;
  } */
.layer {
    background-color: #9594BB;
}

h2{
    color: white;
    font-family: "Irish Grover", system-ui;
}

h1{
    color: white;
    font-family: "Irish Grover", system-ui;
}

.card {
    height: 250px;
}
</style>