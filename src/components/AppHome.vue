// AppHome.vue
<script>
import axios from "axios";
import CardDetails from "./CardDetails.vue";

export default {
  name: "AppHome",
  components: {
    CardDetails,
  },
  data() {
    return {
      products: [],
      groupedProducts: {},
      colors: ["#FF8B64", "#55C2E6", "#FF5E7D", "#4BCF82", "#7335D2", "#5747EA"],
      activeDropdown: null,
      showDetailsModal: false,
      selectedProduct: null,
    };
  },
  mounted() {
    axios
      .get("https://fakestoreapi.com/products")
      .then((response) => {
        this.products = response.data;
        this.groupProductsByCategory();
      })
      .catch((error) => console.error("Error fetching products:", error));
  },
  methods: {
    groupProductsByCategory() {
      this.groupedProducts = this.products.reduce((acc, product) => {
        if (!acc[product.category]) {
          acc[product.category] = [];
        }
        acc[product.category].push(product);
        return acc;
      }, {});

      for (const category in this.groupedProducts) {
        this.groupedProducts[category] = this.groupedProducts[category].slice(0, 4);
      }
    },
    getCategoryColor(category) {
      const index = Object.keys(this.groupedProducts).indexOf(category);
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

<template>
  <div class="main-block">
    <div class="product-list">
      <div class="products">
        <div
          v-for="(categoryProducts, category) in groupedProducts"
          :key="category"
          class="product-category"
        >
          <div class="categoryname" :style="{ backgroundColor: getCategoryColor(category) }">
            <h3 class="px-8 my-0">{{ category }}</h3>
          </div>
          <div class="product-grid">
            <div
              v-for="(product, index) in categoryProducts"
              :key="product.id"
              class="product-card"
              :style="{ backgroundColor: getCategoryColor(category), backgroundImage: `url(${product.image})` }"
            >
              <div class="layer-bg">
                <div class="card-header">
                  <p>{{ product.category }}</p>
                  <span class="menu-icon" @click="toggleDropdown(index)">...</span>
                  <div v-if="activeDropdown === index" class="dropdown">
                    <a class="show" href="#" @click.prevent="showDetails(product)">Show Details</a>
                  </div>
                </div>
                <div class="card-body">
                  <h2 class="price">{{ product.price }}$</h2>
                  <p class="description">{{ product.title }}</p>
                  <div class="card-footer">
                    <span class="stock">InStore {{ product.rating.count }}</span>
                    <span class="rating">Rating {{ product.rating.rate }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <CardDetails v-if="showDetailsModal" :product="selectedProduct" @close="showDetailsModal = false" />
  </div>
</template>

<style lang="scss" scoped>
/* CSS from the original example and additions to match the second code's style */
.main-block {
  font-family: "Rubik-Light";
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background-color: #0f172a;
  color: #fff;
}

.product-list {
  margin-top: 20px;
}

.products {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.product-category {
  margin-bottom: 40px;
  .categoryname {
    font-size: 1.5rem;
    color: white;
    padding: 15px 0;
    border-radius: 10px;
    margin: 20px 0;
    width: 100%;
  }
}

.product-grid {
  display: grid;
  gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}

.product-card {
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
  transition: transform 0.3s ease;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  .layer-bg {
    background-color: #0f172a61;
    height: 350px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  &:hover {
    transform: translateY(-10px);
  }
}

.card-header {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  font-size: 14px;
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

.show:hover {
  color: red !important;
}

.card-body {
  padding: 20px;
  font-size: 16px;
}

.price {
  font-size: 24px;
  margin: 10px 0;
}

.description {
  font-size: 14px;
  color: #bbb;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  color: #bbb;
}
</style>
