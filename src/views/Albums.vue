<template>
  <div class="albums">
    <div class="album-table">
      <div class="album-header">
        <span>Judul Album</span>
      </div>
      <div v-for="album in albums" :key="album.id" class="album-row">
        <router-link :to="'/albums/' + album.id">{{ album.title }}</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue';
import { useAlbumsStore } from '../store/album';

export default defineComponent({
  setup() {
    const albums = ref([]);
    const albumsStore = useAlbumsStore();

    onMounted(async () => {
      await albumsStore.fetchAlbums();
      albums.value = albumsStore.albums;
    });

    return {
      albums,
    };
  },
});
</script>

<style scoped>
.albums {
  max-width: 80%;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  background-color: #ffe4e1;
  color: #333;
  font-size: 18px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
  margin-bottom: 20px;
}

.album-table {
  width: 100%;
  border-collapse: collapse;
}

.album-header {
  background-color: #ff69b4;
  color: #ffffff;
  font-weight: bold;
  padding: 10px;
  border-bottom: 2px solid #ff69b4;
}

.album-row {
  background-color: #ffc0cb;
  color: #333;
  padding: 10px;
  border-bottom: 1px solid #ff69b4;
  transition: background-color 0.3s, transform 0.3s;
}

.album-row a {
  text-decoration: none;
  color: #333;
  font-size: 1.1rem;
}

.album-row:hover {
  background-color: #ff69b4;
  transform: translateY(-2px);
}

.album-header,
.album-row {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
