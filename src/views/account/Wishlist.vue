<template>
  <div class="wishlist-container">
    <!-- Header -->
    <div class="header">
      <div class="title-info">
        <h2>Wishlist</h2>
        <p>Total Barang: {{ filteredWishlist.length }}</p>
      </div>
      <div class="sort">
        <label for="sortBy">Urutkan:</label>
        <select id="sortBy" v-model="sortBy">
          <option value="terbaru">Barang Terbaru</option>
          <option value="terlama">Barang Terlama</option>
          <option value="termurah">Barang Termurah</option>
          <option value="termahal">Barang Termahal</option>
        </select>
      </div>
    </div>

    <!-- Daftar Wishlist -->
    <div v-if="filteredWishlist.length" class="card-grid">
      <div v-for="item in filteredWishlist" :key="item.id" class="card">
        <button class="hapus-button" @click="showConfirm(item.id)">×</button>
        <router-link :to="{ name: 'product-detail', params: { id: item.id } }">
          <img :src="item.image" alt="Foto Produk" class="card-image" />
        </router-link>
        <div class="card-body">
          <h3>{{ item.nama }}</h3>
          <p class="harga">Rp {{ formatRupiah(item.harga) }}</p>
          <small>Ditambahkan: {{ formatTanggal(item.tanggal) }}</small>
        </div>
      </div>
    </div>
    <div v-else class="empty-wishlist">
      <h3>Belum ada barang di wishlist 💔</h3>
      <p>Yuk, temukan produk favoritmu dan tambahkan ke wishlist untuk disimpan!</p>
    </div>

    <!-- Modal Konfirmasi -->
    <div v-if="modalConfirm" class="modal-overlay">
      <div class="modal-box">
        <h3>Konfirmasi</h3>
        <p>Apakah Anda yakin ingin menghapus wishlist ini?</p>
        <div class="modal-actions">
          <button @click="modalConfirm = false">Batal</button>
          <button class="hapus" @click="hapusItem(confirmId)">Hapus</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

const wishlist = ref([]);
const sortBy = ref("terbaru");

onMounted(() => {
  wishlist.value = [
    {
      id: 1,
      nama: "Tas Mclaren",
      harga: 700000,
      tanggal: "2025-06-10",
      image: "https://image.gramedia.net/rs:fit:256:0/plain/https://cdn.gramedia.com/uploads/items/208090415-3.png",
    },
    {
      id: 2,
      nama: "Tas Jansport",
      harga: 200000,
      tanggal: "2025-07-01",
      image: "https://image.gramedia.net/rs:fit:0:0/plain/https://cdn.gramedia.com/uploads/items/208090417-3.png",
    },
    {
      id: 3,
      nama: "Tas Mechanical",
      harga: 450000,
      tanggal: "2025-07-14",
      image: "https://image.gramedia.net/rs:fit:0:0/plain/https://cdn.gramedia.com/uploads/items/HAPPY_RED_BLACK_2HYbVws.png",
    },
  ];
});

const modalConfirm = ref(false);
const confirmId = ref(null);

function showConfirm(id) {
  confirmId.value = id;
  modalConfirm.value = true;
}

function hapusItem(id) {
  wishlist.value = wishlist.value.filter((item) => item.id !== id);
  modalConfirm.value = false;
  confirmId.value = null;
}

const filteredWishlist = computed(() => {
  return [...wishlist.value].sort((a, b) => {
    switch (sortBy.value) {
      case "terbaru":
        return new Date(b.tanggal) - new Date(a.tanggal);
      case "terlama":
        return new Date(a.tanggal) - new Date(b.tanggal);
      case "termurah":
        return a.harga - b.harga;
      case "termahal":
        return b.harga - a.harga;
    }
  });
});

function formatRupiah(angka) {
  return angka.toLocaleString("id-ID");
}

function formatTanggal(tanggal) {
  const date = new Date(tanggal);
  return date.toLocaleDateString("id-ID", {
    day: "2-digit",
    month: "short",
    year: "numeric",
  });
}
</script>

<style scoped>
.wishlist-container {
  padding: 0px 20px 20px 20px;
  background: #fff;
  border-radius: 8px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  margin-bottom: 16px;
}

.title-info h2 {
  font-size: 20px;
}

.sort {
  display: flex;
  align-items: center;
  gap: 8px;
}

.sort select {
  appearance: none;
  background-color: #f5f7fa;
  color: #333;
  border: 1px solid #ccc;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg width='10' height='6' viewBox='0 0 10 6' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1 1l4 4 4-4' stroke='%23999' stroke-width='1.5' fill='none' fill-rule='evenodd'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 10px center;
  background-size: 12px;
}

.card-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}

.card {
  width: 160px;
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  transition: transform 0.2s;
  box-sizing: border-box;
  position: relative;
}

.card:hover {
  transform: translateY(-4px);
}

.card-image {
  width: 100%;
  height: auto;
  aspect-ratio: 4 / 3;
  object-fit: contain;
  background-color: #ffffff;
  display: block;
}

.card-body {
  padding: 10px;
  box-sizing: border-box;
}

.card-body h3 {
  font-size: 14px;
  margin: 0 0 6px;
}

.harga {
  font-weight: bold;
  font-size: 13px;
  margin: 0 0 4px;
}

.hapus-button {
  position: absolute;
  top: 4px;
  right: 6px;
  background: white;
  color: black;
  border: 1px solid #ccc;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  font-size: 14px;
  cursor: pointer;
  padding: 0;
  line-height: 18px;
  text-align: center;
  transition: background-color 0.2s ease;
}

.hapus-button:hover {
  background: #f0f0f0;
}

.empty-wishlist {
  text-align: center;
  padding: 40px 10px;
  color: #777;
  font-size: 15px;
  background: #fdfdfd;
  border: 1px dashed #ccc;
  border-radius: 8px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.modal-box {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 280px;
  max-width: 90%;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
  text-align: center;
}

.modal-box h3 {
  margin: 0 0 10px;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 20px;
}

.modal-actions button {
  padding: 6px 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.modal-actions .hapus {
  background-color: #f44336;
  color: white;
}

.modal-actions button:hover {
  opacity: 0.9;
}

@media (max-width: 768px) {
  .card {
    width: 48%;
  }
}

@media (max-width: 480px) {
  .card {
    flex: 0 0 calc((100% - 32px) / 3);
    width: auto;
  }

  .header {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }

  .sort {
    align-self: stretch;
    justify-content: space-between;
    width: 100%;
  }
}
</style>
