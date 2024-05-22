<template>
    <App>
      <template v-slot:navbar>
        <div class="additional-navbar-content">
          <!-- Tambahkan konten tambahan di sini -->
          <p>Ini Adalah Halaman Konten Ellsya</p>
        </div>
      </template>
      <template v-slot:footer>
        <div class="footer">
          <!-- Tambahkan konten footer di sini -->
          <p>Silahkan Menggunakan</p>
        </div>
      </template>
    </App>
  </template>
  
  <script>
  import App from './App.vue';
  
  export default {
    components: {
      App
    }
  };
  </script>
  
  <style>
  .additional-navbar-content {
    color: white;
    text-align: center;
    background-color: #444;
    padding: 10px;
  }
  
  .footer {
    background-color: #333;
    color: white;
    text-align: center;
    padding: 10px;
    margin-top: 20px;
  }
  </style>
  