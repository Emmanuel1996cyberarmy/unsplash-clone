<template>
  <div id="app">
    <header>
      <SearchBar @search="handleSearch" />
    </header>
    <main>
      <LandingScreen v-if="!searchTerm" @open-modal="openModal" />
      <SearchResults v-else :searchTerm="searchTerm" @open-modal="openModal" />
    </main>
    <button v-if="selectedImage" class="close-button" @click="closeModal">
      <svg
        class="w-4 h-4 text-gray-800 dark:text-white"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        fill="none"
        viewBox="0 0 24 24"
      >
        <path
          stroke="currentColor"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M6 18 17.94 6M18 18 6.06 6"
        />
      </svg>
    </button>
    <ImageModal
      v-if="selectedImage"
      :image="selectedImage"
      @close="closeModal"
    />
  </div>
</template>

<script>
import SearchBar from "./components/SearchBar.vue";
import LandingScreen from "./components/LandingScreen.vue";
import SearchResults from "./components/SearchResults.vue";
import ImageModal from "./components/ImageModal.vue";

export default {
  name: "App",
  components: {
    SearchBar,
    LandingScreen,
    SearchResults,
    ImageModal,
  },
  data() {
    return {
      searchTerm: "",
      selectedImage: null,
    };
  },
  methods: {
    handleSearch(term) {
      this.searchTerm = term;
    },
    openModal(image) {
      console.log("openModal triggered with image:", image);
      this.selectedImage = image;
    },

    closeModal() {
      this.selectedImage = null;
    },
  },
};
</script>


<style lang="scss" scoped>
:root {
  --primary-color: #333;
  --secondary-color: #dde3ea;
  --background-color: #f4f4f4;
  --overlay-color: rgba(0, 0, 0, 0.5);
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html,
body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

body {
  font-family: Arial, sans-serif;
  background-color: var(--background-color);
  color: var(--primary-color);
}

#app {
  width: 100%;
  margin: 0 auto;
}

header {
  background-color: #dde3ea;
  padding: 95px;
  width: 100%;
  margin: 0;
  position: relative;
  z-index: 2;
}

main {
  position: relative;
  z-index: 2;
}

.close-button {
  position: fixed;
  top: 70px;
  right: 170px;
  font-size: 24px;
  color: #9299a1;
  background: none;

  border: none;
  width: 30px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  z-index: 3000;
}
</style>
