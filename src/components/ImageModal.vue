<template>
  <div class="image-modal" @click.self="$emit('close')">
    <div class="modal-content">
      <img
        v-if="image?.urls?.full"
        :src="image.urls.full"
        :alt="image.alt_description"
      />
      <!-- <button class="close-button" @click="$emit('close')">&times;</button> -->
      <div class="image-info" v-if="image">
        <h3>{{ image?.user?.name }}</h3>
        <p v-if="image.location?.name">{{ image.location.name }}</p>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "ImageModal",
  props: {
    image: {
      type: Object,
      required: true,
    },
  },
};
</script>
  
<style lang="scss" scoped>
.image-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2000;
}

.modal-content {
  position: relative;
  max-width: 90%;
  max-height: 90%;
  overflow: hidden;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);

  img {
    display: block;
    width: 100%;
    height: auto;
    max-height: calc(90vh - 100px);
    object-fit: contain;
  }
}

.image-info {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 15px;
  background-color: white;

  h3 {
    margin: 0 0 5px 0;
    font-size: 18px;
    color: #24344e;
  }

  p {
    margin: 0;
    font-size: 14px;
    color: #9299a1;
  }

  .description {
    margin-top: 10px;
    font-style: italic;
  }
}

@media (max-width: 768px) {
  .modal-content {
    width: 95%;
    max-height: 95%;
  }

  .image-info {
    position: relative;
    background-color: black;
  }
}
</style>
