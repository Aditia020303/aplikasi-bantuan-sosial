<template>
  <div id="app">
    <FormBantuan v-if="!isPreview" @onSubmit="handleFormSubmit" />
    <PreviewData v-else :formData="formData" @back="isPreview = false" />
  </div>
</template>

<script>
import FormBantuan from "./components/FormBantuan.vue";
import PreviewData from "./components/PreviewData.vue";

export default {
  components: {
    FormBantuan,
    PreviewData,
  },
  data() {
    return {
      isPreview: false,
      formData: null,
    };
  },
  methods: {
    handleFormSubmit(data) {
      this.formData = data;
      this.isPreview = true;
    },
  },
};
</script>

<style>
/* Reset dasar */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Styling untuk keseluruhan halaman */
#app {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
}

/* Styling untuk form container */
form {
  background: #fff;
  padding: 2rem;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  width: 100%;
  max-width: 500px;
}

h1 {
  font-size: 1.8rem;
  color: #333;
  margin-bottom: 1rem;
  text-align: center;
}

label {
  display: block;
  font-weight: bold;
  margin-top: 1rem;
  color: #555;
}

input[type="text"],
input[type="number"],
input[type="file"],
select,
button {
  width: 100%;
  padding: 0.8rem;
  margin-top: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

input[type="file"] {
  padding: 0.3rem;
}

button {
  background-color: #4caf50;
  color: white;
  font-weight: bold;
  border: none;
  cursor: pointer;
  margin-top: 1.5rem;
}

button:hover {
  background-color: #45a049;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

/* Pesan sukses atau error */
div[status="success"],
div[status="error"] {
  padding: 1rem;
  border-radius: 4px;
  margin-top: 1rem;
  text-align: center;
  color: white;
}

div[status="success"] {
  background-color: #4caf50;
}

div[status="error"] {
  background-color: #f44336;
}

/* Link ke halaman preview */
router-link {
  display: block;
  margin-top: 1rem;
  text-align: center;
  color: #007bff;
  font-weight: bold;
  text-decoration: none;
}

router-link:hover {
  text-decoration: underline;
}

/* Styling untuk halaman preview */
.preview-container {
  background: #fff;
  padding: 2rem;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 500px;
  width: 100%;
  text-align: left;
}

.preview-container p {
  margin-bottom: 0.8rem;
}

.preview-container p strong {
  font-weight: bold;
  color: #333;
}

.preview-container button {
  display: block;
  margin: 2rem auto 0;
  padding: 0.8rem 2rem;
}
</style>
