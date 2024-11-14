<template>
  <div>
    <h1>Preview Data Penerima Bantuan Sosial</h1>
    <div class="preview-container">
      <div class="preview-item">
        <strong>Nama:</strong>
        <span>{{ formData.nama || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>NIK:</strong>
        <span>{{ formData.nik || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>Umur:</strong>
        <span>{{ formData.umur || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>Provinsi:</strong>
        <span>{{ formData.provinsi?.name || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>Kabupaten/Kota:</strong>
        <span>{{ formData.kabupaten?.name || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>Kecamatan:</strong>
        <span>{{ formData.kecamatan?.name || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>Kelurahan/Desa:</strong>
        <span>{{ formData.kelurahan?.name || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>RT:</strong>
        <span>{{ formData.rt || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>RW:</strong>
        <span>{{ formData.rw || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>Alamat Lengkap:</strong>
        <span>{{ formData.alamatLengkap || "Belum diisi" }}</span>
      </div>

      <div class="preview-item">
        <strong>Foto KTP:</strong>
        <img
          v-if="formData.fotoKTP"
          :src="fotoKTPUrl"
          alt="Foto KTP"
          class="preview-image"
        />
        <span v-else>Belum ada foto KTP</span>
      </div>

      <div class="preview-item">
        <strong>Foto Kartu Keluarga:</strong>
        <img
          v-if="formData.fotoKK"
          :src="fotoKKUrl"
          alt="Foto Kartu Keluarga"
          class="preview-image"
        />
        <span v-else>Belum ada foto KK</span>
      </div>
    </div>

    <button @click="$emit('back')">Kembali ke Form</button>
  </div>
</template>

<script>
export default {
  props: ["formData"],
  computed: {
    fotoKTPUrl() {
      return this.formData.fotoKTP
        ? URL.createObjectURL(this.formData.fotoKTP)
        : "";
    },
    fotoKKUrl() {
      return this.formData.fotoKK
        ? URL.createObjectURL(this.formData.fotoKK)
        : "";
    },
  },
};
</script>

<style scoped>
.preview-container {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.preview-item {
  display: flex;
  justify-content: space-between;
  font-size: 16px;
}

strong {
  font-weight: bold;
}

.preview-image {
  max-width: 150px;
  max-height: 150px;
  object-fit: cover;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>
