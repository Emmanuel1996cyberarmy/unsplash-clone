<template>
  <div class="search-bar">
    <svg
      class="search-icon"
      aria-hidden="true"
      xmlns="http://www.w3.org/2000/svg"
      width="20"
      height="20"
      fill="none"
      viewBox="0 0 24 24"
    >
      <path
        stroke="currentColor"
        stroke-linecap="round"
        stroke-width="2"
        d="m21 21-3.5-3.5M17 10a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z"
      />
    </svg>
    <input
      type="text"
      v-model="searchInput"
      @keyup.enter="handleSearch"
      @input="handleInputChange"
      placeholder="Search for photo"
    />
    <!-- <button @click="handleSearch">Search</button> -->
  </div>
</template>
  
  <script>
export default {
  name: "SearchBar",
  props: {
    searchTerm: {
      type: String,
      required: false,
    },
  },
  data() {
    return {
      searchInput: "",
    };
  },
  methods: {
    handleSearch() {
      this.$emit("search", this.searchInput.trim());
    },
    handleInputChange() {
      this.$emit("search", this.searchInput.trim());
    },
  },
};
</script>
  
  <style lang="scss" scoped>
.search-bar {
  max-width: 1000px;
  margin: 0 auto 20px;
  position: relative;
  padding-right: 10%;

  input {
    width: 100%;
    padding: 20px 20px 15px 55px;
    font-size: 18px;
    border: none;
    border-radius: 5px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    outline: none;
    margin-left: 15px;

    &::placeholder {
      color: #575a68;
    }
  }

  .search-icon {
    position: absolute;
    left: 30px;
    top: 53%;
    transform: translateY(-50%);
    width: 20px;
    height: 18px;
    fill: none;
    stroke: #9b9fa2;
    color: #9b9fa2;
  }
}
</style>