<template>
  <div class="background">
    <img src="https://img.freepik.com/free-photo/pink-sky-background-with-crescent-moon_53876-129048.jpg?w=996&t=st=1715252408~exp=1715253008~hmac=b1fff2fb0b7c0d3f22ce41a6792152f37483f5da2fd82e4e16511ad0c694c915"/>
  </div>
  <div class="container mt-4">
    <div class="mb-3">
      <input v-model="newActivity" type="text" class="form-control" placeholder="Tambah kegiatan">
      <button @click="addActivity" class="btn btn-primary mt-2" style="margin-left: 5px;">Tambah</button>
    </div>
    <button @click="toggleFilter" :class="{ 'btn-dark': filterActive, 'btn-secondary': !filterActive }" class="btn mb-3">
      {{ filterActive ? 'Tampilkan Semua' : 'Tampilkan Belum Selesai' }}
    </button>
    <table class="table">
      <thead>
        <tr class="table-header">
          <th>Kegiatan</th>
          <th>Status</th>
          <th>Edit</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(activity, index) in filteredActivities" :key="index" class="table-row">
          <td :class="{ 'text-decoration-line-through': activity.completed }" class="table-data">
            {{ activity.name }}
          </td>
          <td class="table-data">
            <input type="checkbox" v-model="activity.completed" @change="toggleStatus(activity)">
          </td>
          <td class="table-data">
            <button @click="editActivity(activity)" class="btn btn-info">Edit</button> <!-- Tombol edit -->
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import Swal from 'sweetalert2';

const newActivity = ref('');
const filterActive = ref(false);
const activities = ref([
  { name: 'Membaca buku', completed: false },
  { name: 'Menulis laporan', completed: true },
  { name: 'Bersantai di taman', completed: true },
]);

const filteredActivities = computed(() => {
  if (filterActive.value) {
    return activities.value.filter(activity => !activity.completed);
  } else {
    return activities.value;
  }
});

const addActivity = () => {
  if (newActivity.value.trim() === '') return;
  activities.value.push({ name: newActivity.value, completed: false });
  newActivity.value = '';
  Swal.fire({
    icon: 'success',
    title: 'Kegiatan Ditambahkan!',
    showConfirmButton: false,
    timer: 1500
  });
};

const toggleStatus = (activity) => {
  if (activity.completed) {
    Swal.fire({
      icon: 'success',
      title: 'Kegiatan Selesai!',
      showConfirmButton: false,
      timer: 1500
    });
  } else {
    Swal.fire({
      icon: 'info',
      title: 'Kegiatan Belum Selesai!',
      showConfirmButton: false,
      timer: 1500
    });
  }
};

const toggleFilter = () => {
  filterActive.value = !filterActive.value;
};

const editActivity = (activity) => {
  Swal.fire({
    title: 'Edit Kegiatan',
    input: 'text',
    inputValue: activity.name,
    showCancelButton: true,
    confirmButtonText: 'Simpan',
    cancelButtonText: 'Batal',
    inputValidator: (value) => {
      if (!value) {
        return 'Kolom tidak boleh kosong!';
      }
    }
  }).then((result) => {
    if (result.isConfirmed) {
      activity.name = result.value;
      Swal.fire({
        icon: 'success',
        title: 'Kegiatan Diperbarui!',
        showConfirmButton: false,
        timer: 1500
      });
    }
  });
};
</script>

<style>
.container input, button, table{
  max-width: 100%;
  margin-left: -50%;
  z-index: 1;
}
.text-decoration-line-through {
  text-decoration: line-through;
}
.mb-3{
  display: flex;
}
.btn-secondary{
  margin-left: -50%;
}

.background img{
  position: fixed;
  top: 0;
  left: 0;
  min-height: 90%;
  min-width: 1500px;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -2;
  object-fit: cover;
  -webkit-background-size:cover; -moz-background-size:cover; -o-background-size:cover; background-size: cover;
}
.table{
  border: 1px solid black;
  max-width: 100%;
  max-height: 100%;
}

tr th {
  background-color: gray;
}

.table-row {
  background-color: transparent !important;
}

.table-data {
  color: transparent;
}
</style>
