<template>
  <div class="landing-screen">
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
          <p class="author">{{ photo.user.name }}</p>
          <p class="location" v-if="photo.location.name">
            {{ photo.location.name }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>
  
  <script>
import axios from "axios";

export default {
  name: "LandingScreen",
  data() {
    return {
      photos: [],
    };
  },
  mounted() {
    this.fetchPhotos().then(() => {
      this.setGridItemHeight();
      window.addEventListener("resize", this.setGridItemHeight);
    });
  },
  methods: {
    handleImageClick(photo) {
      // Better name for clarity
      console.log("Image clicked: ", photo); // Debugging to ensure click works
      this.$emit("open-modal", photo); // Emit the event to the parent
    },
    setGridItemHeight() {
      this.$nextTick(() => {
        const items = this.$el.querySelectorAll(".photo-item");
        items.forEach((item) => {
          const img = item.querySelector("img");
          if (img && img.complete) {
            const rowSpan = Math.ceil(img.height / 10);
            item.style.gridRowEnd = `span ${rowSpan}`;
          } else {
            item.style.gridRowEnd = `span 30`;
          }
        });
      });
    },
    async fetchPhotos() {
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
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.setGridItemHeight);
  },
};
</script>
  
  <style lang="scss" scoped>
.landing-screen {
  position: relative;
}

.photo-grid {
  display: grid;
  grid-template-columns: repeat(3, 250px);
  grid-auto-rows: 35px;
  gap: 50px;
  max-width: fit-content;
  margin: 0 auto;
  z-index: 9;
  margin-top: -47px;
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