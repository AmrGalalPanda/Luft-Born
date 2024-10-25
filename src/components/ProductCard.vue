<template>
  <div
    class="upper-card mb-md-33 mb-21 img-20-xxl img-33-lg img-49-sm img-100 mr-6-xxl ml-6-xxl"
    :style="{ backgroundColor: getBackgroundColor(index) }"
    v-for="(product, index) in products"
    :key="product.id"
  >
    <div class="card mt-md-42 mt-38 pb-33">
      <div class="card-header pt-16">
        <p class="font-18-mid pl-md-30 pl-24">{{ product.category }}</p>
        <span class="menu-icon font-39-mid pb-8 pr-16" @click="toggleDropdown(index)">...</span>
        <div v-if="activeDropdown === index" class="dropdown">
          <a class="show" href="#" @click="showDetails(product)">Show Details</a>
        </div>
      </div>
      <div class="card-body d-md-flex justify-between">
        <p class="card-price my-0 img-90-md font-39-mid pl-md-30 pl-24 mb-md-9">{{ product.price }}$</p>
        <div class="d-flex justify-between img-100-md img-42 w-100 my-6">
          <h3 class="card-title my-0 font-12-mid img-49-md pl-md-30 px-6">{{ product.title }}</h3>
          <div class="card-rating my-0 font-12-l img-20 pr-16 text-center d-none">
            <span class="d-block">InStore</span>
            <span class="d-block">{{ product.rating.count }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
  <CardDetails v-if="showDetailsModal" :product="selectedProduct" @close="showDetailsModal = false" />
</template>

<script>
import CardDetails from './CardDetails.vue';

export default {
  name: "ProductCard",
  components: {
    CardDetails,
  },
  props: {
    products: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      colors: ['#FF8B64', '#55C2E6', '#FF5E7D', '#4BCF82', '#7335D2', '#5747EA'],
      activeDropdown: null,
      showDetailsModal: false,
      selectedProduct: null,
    };
  },
  methods: {
    getBackgroundColor(index) {
      return this.colors[index % this.colors.length];
    },
    toggleDropdown(index) {
      this.activeDropdown = this.activeDropdown === index ? null : index;
    },
    showDetails(product) {
      this.selectedProduct = product;
      this.showDetailsModal = true;
      this.activeDropdown = null;
    },
  },
};
</script>

<style lang="scss" scoped>
.upper-card {
  width: 255px;
  border-radius: 15px;
  max-height: 243px;
  overflow: hidden;
  margin: auto;
}

.card {
  border-radius: 15px;
  transition: transform 0.3s;
  height: 100%;
}

.card-header {
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
}

.menu-icon {
  cursor: pointer;
}

.dropdown {
  position: absolute;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  padding: 10px;
  margin-top: 5px;
  top: 55%;
  right: 4%;
}

.dropdown a {
  text-decoration: none;
  color: #333;
}

.card-title {
  margin: 10px 0;
}

.show:hover {
  color: red !important;
}

@media screen and (max-width: 576px) {
  .upper-card {
    width: 327px;
  }
}

@media screen and (max-width: 992px) {
  .font-12-mid {
    font-size: 10px;
  }

  .upper-card {
    width: 100%;
    margin-left: 0 !important;
    margin-right: 0 !important;
  }

  .d-md-flex {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: center;
  }

  .d-none {
    display: none;
  }
}
</style>
