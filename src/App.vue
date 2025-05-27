<template>
  <div class="container">
    <h1>☕ RGT Coffee Online</h1>

    <!-- Form Input Nama -->
    <input v-model="namaPelanggan" type="text" placeholder="Masukkan nama anda" />

    <!-- Daftar Menu Kopi -->
    <h2>Daftar Menu</h2>
    <ul>
      <li v-for="menu in menus" :key="menu.id">
        <img :src="menu.imageUrl" :alt="menu.name" width="80" />
        {{ menu.name }} - Rp {{ menu.price }}
        <button @click="tambahPesanan(menu)">Pesan</button>
      </li>
    </ul>

    <!-- Checkbox Topping -->
    <h3>Pilih Topping:</h3>
    <label><input type="checkbox" value="Coklat" v-model="topping"> Coklat</label>
    <label><input type="checkbox" value="Keju" v-model="topping"> Keju</label>

    <!-- Radio Button Metode Pengambilan -->
    <h3>Metode Pengambilan:</h3>
    <label><input type="radio" value="Antar" v-model="metode"> Antar</label>
    <label><input type="radio" value="Ambil di Tempat" v-model="metode"> Ambil di Tempat</label>

    <!-- Form Catatan -->
    <textarea v-model="catatan" placeholder="Catatan tambahan..."></textarea>

    <!-- Pesanan -->
    <h2>Pesanan Anda</h2>
    <p v-if="pesanan.length === 0">Belum ada pesanan, yuk pilih kopinya dulu!</p>
    <ul v-else>
      <li v-for="(item, index) in pesanan" :key="index">
        {{ item.name }} - Rp {{ item.price }}
      </li>
    </ul>

    <!-- Total -->
    <h3>Total: Rp {{ totalHarga }}</h3>

    <!-- Tombol Submit -->
    <button @click="kirimPesanan">Kirim Pesanan</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const namaPelanggan = ref('')
const menus = ref([
  { id: 1, name: 'Kopi Hitam', price: 12000, imageUrl: 'https://via.placeholder.com/80?text=Hitam' },
  { id: 2, name: 'Kopi Susu', price: 15000, imageUrl: 'https://via.placeholder.com/80?text=Susu' },
  { id: 3, name: 'Cappuccino', price: 18000, imageUrl: 'https://via.placeholder.com/80?text=Cappuccino' }
])

const pesanan = ref([])
const topping = ref([])
const metode = ref('')
const catatan = ref('')

const tambahPesanan = (menu) => {
  pesanan.value.push(menu)
}

const totalHarga = computed(() => {
  return pesanan.value.reduce((total, item) => total + item.price, 0)
})

const kirimPesanan = () => {
  if (pesanan.value.length === 0) {
    alert('Pesanan masih kosong!')
    return
  }

  alert(`Terima kasih ${namaPelanggan.value}, pesanan anda akan segera diproses.\nMetode: ${metode.value}\nTopping: ${topping.value.join(', ')}\nCatatan: ${catatan.value}`)
  
  // reset data
  pesanan.value = []
  topping.value = []
  metode.value = ''
  catatan.value = ''
  namaPelanggan.value = ''
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 20px auto;
  font-family: sans-serif;
}
h1, h2, h3 {
  margin-bottom: 10px;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin-bottom: 8px;
}
input, textarea, button {
  margin: 8px 0;
  display: block;
}
img {
  margin-right: 8px;
  vertical-align: middle;
}
</style>
