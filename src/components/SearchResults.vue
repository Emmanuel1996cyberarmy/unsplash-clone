<template>
  <div class="search-results">
    <h2 v-if="loading">{{ loadingMessage }}</h2>
    <h2 v-else-if="!loading && searchTerm">
      Search Results for <span>"{{ searchTerm }}"</span>
    </h2>
    <h2 v-else>No results found. Displaying default photos.</h2>
    <div class="photo-grid">
      <div v-for="photo in photos" :key="photo.id" class="photo-item">
        <img
          v-if="photo.loaded"
          :src="photo.urls.regular"
          :alt="photo.alt_description"
          @click="handleImageClick(photo)"
        />
        <div v-else class="skeleton"></div>
        <div class="photo-info">
          <p class="author">{{ photo?.user?.name }}</p>
          <p class="location" v-if="photo?.location?.name">
            {{ photo?.location?.name }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
  
  <script>
import axios from "axios";

export default {
  name: "SearchResults",
  props: {
    searchTerm: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      photos: [],
      loading: false,
    };
  },
  computed: {
    loadingMessage() {
      return `Searching for "${this.searchTerm}"`;
    },
  },
  watch: {
    searchTerm: {
      immediate: true,
      handler: "fetchPhotos",
    },
  },
  methods: {
    handleImageClick(photo) {
      console.log("Image clicked: ", photo);
      this.$emit("open-modal", photo);
    },
    async fetchPhotos() {
      this.loading = true;
      this.photos = [];
      if (this.searchTerm.trim()) {
        try {
          const response = await axios.get(
            "https://api.unsplash.com/search/photos",
            {
              params: {
                query: this.searchTerm,
                per_page: 8,
                client_id: import.meta.env.VITE_UNSPLASH_ACCESS_KEY,
              },
            }
          );
          console.log(response, "@@response searching");
          this.photos = response.data.results.map((photo) => ({
            ...photo,
            loaded: false,
          }));
          this.loading = false;
          this.preloadImages();
        } catch (error) {
          console.error("Error fetching photos:", error);
          this.photos = [];
          this.error = "Failed to fetch photos";
        }
      } else {
        this.photos = await this.fetchDefaultPhotos();
        this.loading = false;
      }
    },

    preloadImages() {
      this.photos.forEach((photo, index) => {
        const img = new Image();
        img.onload = () => {
          this.photos[index].loaded = true;
        };
        img.src = photo.urls.regular;
      });
    },
    async fetchDefaultPhotos() {
      try {
        const response = await axios.get(
          "https://api.unsplash.com/photos/random",
          {
            params: {
              query: "African",
              count: 8,
              client_id: import.meta.env.VITE_UNSPLASH_ACCESS_KEY,
            },
          }
        );
        console.log(response, "@@response");
        this.photos = response.data.map((photo) => ({
          ...photo,
          loaded: false,
        }));
        this.preloadImages();
      } catch (error) {
        console.error("Error fetching photos:", error);
        this.photos = [];
      }
    },
  },
};
</script>
  
  <style lang="scss" scoped>
.search-results {
  margin-top: -100px;
  position: relative;
}

h2 {
  margin-bottom: 10px;
  max-width: 700px;
  text-align: center;
  padding: 5px 22px 0 0;
  color: #29385a;
}

span {
  color: #6c7d8e;
}

.photo-grid {
  display: grid;
  grid-template-columns: repeat(3, 250px);
  grid-auto-rows: 35px;
  gap: 50px;
  max-width: fit-content;
  margin: 0 auto;
  z-index: 9;
  // margin-top: -60px;
  position: relative;
}

.photo-item {
  position: relative;
  overflow: hidden;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    position: relative;
    z-index: 2;
  }

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.4);
    z-index: 1;
    transition: background 0.3s ease;
  }
}

.photo-item:nth-child(1) {
  grid-row: span 3;
}

.photo-item:nth-child(2) {
  grid-row: span 5;
}

.photo-item:nth-child(3) {
  grid-row: span 4;
}

.photo-item:nth-child(4) {
  grid-row: span 5;
}

.photo-item:nth-child(5) {
  grid-row: span 5;
}

.photo-item:nth-child(6) {
  grid-row: span 5;
}
.photo-item:nth-child(7) {
  grid-row: span 4;
}
.photo-item:nth-child(8) {
  grid-row: span 4;
}

.photo-info {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 10px;
  z-index: 2;

  color: white;

  .author {
    font-weight: bold;
    margin-bottom: 5px;
  }

  .location {
    font-size: 0.9em;
  }
}

.skeleton {
  width: 300px;
  height: 300px;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite;
}

@keyframes loading {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
  }
}

@media (max-width: 768px) {
  .photo-grid {
    grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
    gap: 15px;
  }
}

@media (max-width: 480px) {
  .photo-grid {
    grid-template-columns: 1fr;
    gap: 10px;
  }
}
</style>