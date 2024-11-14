<template>
  <div>
    <div class="container">
      <div class="logo-container">
        <img src="@/assets/logo.PNG" alt="Logo" class="logo" />
      </div>
      <h1 class="title">Formulir Penerima Bantuan Sosial</h1>
    </div>
    <form @submit.prevent="submitForm">
      <label>Nama:</label>
      <input type="text" v-model="form.nama" required />

      <label>NIK:</label>
      <input type="number" v-model="form.nik" required />

      <label>Nomor Kartu Keluarga:</label>
      <input type="text" v-model="form.nomorKK" required />

      <label>Foto KTP (Maks 2MB, JPG/JPEG/PNG/BMP):</label>
      <input
        type="file"
        accept=".jpg,.jpeg,.png,.bmp"
        @change="handleFileChange('ktp')"
        required
      />

      <label>Foto Kartu Keluarga (Maks 2MB, JPG/JPEG/PNG/BMP):</label>
      <input
        type="file"
        accept=".jpg,.jpeg,.png,.bmp"
        @change="handleFileChange('kk')"
        required
      />

      <label>Umur:</label>
      <input type="number" v-model="form.umur" min="1" required />

      <label>Jenis Kelamin:</label>
      <select v-model="form.jenisKelamin" required>
        <option value="LakiLaki">Laki-laki</option>
        <option value="Perempuan">Perempuan</option>
      </select>

      <label>Provinsi:</label>
      <select v-model="form.provinsi" @change="loadKabupaten" required>
        <option
          v-for="prov in provinsi"
          :key="prov.id"
          :value="{ id: prov.id, name: prov.name }"
        >
          {{ prov.name }}
        </option>
      </select>

      <label>Kabupaten/Kota:</label>
      <select v-model="form.kabupaten" @change="loadKecamatan" required>
        <option
          v-for="kab in kabupaten"
          :key="kab.id"
          :value="{ id: kab.id, name: kab.name }"
        >
          {{ kab.name }}
        </option>
      </select>

      <label>Kecamatan:</label>
      <select v-model="form.kecamatan" @change="loadKelurahan" required>
        <option
          v-for="kec in kecamatan"
          :key="kec.id"
          :value="{ id: kec.id, name: kec.name }"
        >
          {{ kec.name }}
        </option>
      </select>

      <label>Kelurahan/Desa:</label>
      <select v-model="form.kelurahan" required>
        <option
          v-for="kel in kelurahan"
          :key="kel.id"
          :value="{ id: kel.id, name: kel.name }"
        >
          {{ kel.name }}
        </option>
      </select>

      <label>RT:</label>
      <input type="number" v-model="form.rt" required />

      <label>RW:</label>
      <input type="number" v-model="form.rw" required />

      <label>Alamat Lengkap:</label>
      <textarea
        v-model="form.alamatLengkap"
        rows="3"
        maxlength="255"
        required
      ></textarea>

      <label>Penghasilan Sebelum Pandemi:</label>
      <input type="number" v-model="form.penghasilanSebelum" required />

      <label>Penghasilan Setelah Pandemi:</label>
      <input type="number" v-model="form.penghasilanSetelah" required />

      <label>Alasan Membutuhkan Bantuan:</label>
      <select v-model="form.alasanBantuan" required>
        <option value="kehilangan pekerjaan">Kehilangan Pekerjaan</option>
        <option value="kepala keluarga">Kepala Keluarga</option>
        <option value="fakir miskin">Tergolong Fakir/Miskin</option>
        <option value="lainnya">Lainnya</option>
      </select>

      <div v-if="form.alasanBantuan === 'lainnya'">
        <label>Alasan Lainnya:</label>
        <input type="text" v-model="form.alasanLainnya" />
      </div>

      <div class="checkbox-container">
        <input
          type="checkbox"
          v-model="form.pernyataan"
          id="pernyataan"
          required
        />
        <label for="pernyataan">
          Saya menyatakan bahwa data yang diisikan adalah benar dan siap
          mempertanggungjawabkan apabila ditemukan ketidaksesuaian dalam data
          tersebut.
        </label>
      </div>

      <button @click="debouncedSubmit" :disabled="isSubmitting">Kirim</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        nama: "",
        nik: "",
        nomorKK: "",
        fotoKTP: null,
        fotoKK: null,
        umur: "",
        jenisKelamin: "",
        provinsi: null,
        kabupaten: null,
        kecamatan: null,
        kelurahan: null,
        rt: "",
        rw: "",
        alamatLengkap: "",
        penghasilanSebelum: "",
        penghasilanSetelah: "",
        alasanBantuan: "",
        alasanLainnya: "",
        pernyataan: false, // status checkbox
      },
      provinsi: [],
      kabupaten: [],
      kecamatan: [],
      kelurahan: [],
    };
  },
  methods: {
    async loadProvinsi() {
      const response = await fetch(
        "https://www.emsifa.com/api-wilayah-indonesia/api/provinces.json"
      );
      this.provinsi = await response.json();
    },
    async loadKabupaten() {
      this.kabupaten = [];
      this.kecamatan = [];
      this.kelurahan = [];
      const response = await fetch(
        `https://www.emsifa.com/api-wilayah-indonesia/api/regencies/${this.form.provinsi.id}.json`
      );
      this.kabupaten = await response.json();
    },
    async loadKecamatan() {
      this.kecamatan = [];
      this.kelurahan = [];
      const response = await fetch(
        `https://www.emsifa.com/api-wilayah-indonesia/api/districts/${this.form.kabupaten.id}.json`
      );
      this.kecamatan = await response.json();
    },
    async loadKelurahan() {
      this.kelurahan = [];
      const response = await fetch(
        `https://www.emsifa.com/api-wilayah-indonesia/api/villages/${this.form.kecamatan.id}.json`
      );
      this.kelurahan = await response.json();
    },
    handleFileChange(type) {
      const fileInput = event.target;
      const file = fileInput.files[0];

      if (file.size > 2 * 1024 * 1024) {
        alert("File terlalu besar. Maksimal 2MB.");
        return;
      }

      if (!["image/jpeg", "image/png", "image/bmp"].includes(file.type)) {
        alert("Format file tidak valid. Harus JPG, JPEG, PNG, atau BMP.");
        return;
      }

      if (type === "ktp") {
        this.form.fotoKTP = file;
      } else if (type === "kk") {
        this.form.fotoKK = file;
      }
    },
    submitForm() {
      // Emit data ke parent
      this.$emit("onSubmit", this.form);
    },
  },
  mounted() {
    this.loadProvinsi();
  },
};
</script>

<style scoped>
.checkbox-container {
  display: flex;
  align-items: center;
  gap: 10px;
}

button[disabled] {
  background-color: #ccc;
}

textarea {
  width: 100%;
  padding: 0.8rem;
  margin-top: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  box-sizing: border-box;
}

textarea {
  resize: vertical; /* Mengizinkan pengguna untuk mengubah ukuran secara vertikal */
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 30vh;
  text-align: center;
  margin: 0;
}

.logo-container {
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.logo {
  width: 200px; /* Ukuran logo */
  height: auto;
}
</style>
