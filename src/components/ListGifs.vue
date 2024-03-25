<template>
  <div>
    <h1>Busca de GIFs</h1>
    <input
      v-model="searchQuery"
      @keyup.enter="searchGifs"
      placeholder="Pesquisar GIFs"
    />
    <div v-if="loading" class="loading">Carregando...</div>
    <div v-else class="gif-container">
      <div v-for="(gif, index) in gifs" :key="index" @click="viewGif(gif)">
        <img :src="gif.images.fixed_height.url" :alt="gif.title" />
      </div>
      <div v-if="loadingMore" class="loading">Carregando mais...</div>
    </div>
    <gif-modal
      :show-modal="isModalOpen"
      :selected-gif="selectedGif"
      @update:show-modal="isModalOpen = $event"
    />
  </div>
</template>

<script>
import axios from "axios";
import GifModal from "components/GifModal.vue";

export default {
  components: {
    GifModal,
  },
  data() {
    return {
      searchQuery: "",
      gifs: [],
      loading: false,
      loadingMore: false,
      offset: 0,
      isModalOpen: false,
      selectedGif: null,
    };
  },
  methods: {
    async searchGifs() {
      this.offset = 0;
      this.gifs = [];
      this.loading = true;
      await this.fetchGifs();
      this.loading = false;
    },
    async fetchGifs() {
      try {
        const response = await axios.get(
          `https://api.giphy.com/v1/gifs/search?api_key=4B8NmPS1lcsNZ4tRfBYQci3dWfQXMf0y&q=${this.searchQuery}&limit=10&offset=${this.offset}`
        );
        this.gifs = this.gifs.concat(response.data.data);
        this.offset += 10;
      } catch (error) {
        console.error("Error fetching GIFs:", error);
      }
    },
    async fetchPopularGifs() {
      try {
        const response = await axios.get(
          `https://api.giphy.com/v1/gifs/trending?api_key=4B8NmPS1lcsNZ4tRfBYQci3dWfQXMf0y&limit=10`
        );
        this.gifs = response.data.data;
      } catch (error) {
        console.error("Error fetching popular GIFs:", error);
      }
    },
    async viewGif(gif) {
      this.selectedGif = gif;
      this.isModalOpen = true; // Abrir o modal ao selecionar um GIF
    },
    closeModal() {
      this.isModalOpen = false; // Fechar o modal quando o botão "Fechar" é clicado
    },
    async loadMore() {
      this.loadingMore = true;
      await this.fetchGifs();
      this.loadingMore = false;
    },
  },
  created() {
    this.fetchPopularGifs(); // Chama a função para buscar GIFs populares ao carregar a página
    window.addEventListener("scroll", () => {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        this.loadMore();
      }
    });
  },
};
</script>

<style scoped>
.gif-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-gap: 10px;
}
.loading {
  text-align: center;
  margin-top: 20px;
}
</style>
