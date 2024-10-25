<script>
import axios from "axios";

export default {
  name: "AppHome",
  data() {
    return {
      products: [],
      groupedProducts: {},
      colors: ["#FF8B64", "#55C2E6", "#FF5E7D", "#4BCF82", "#7335D2", "#5747EA"],
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
              v-for="product in categoryProducts"
              :key="product.id"
              class="product-card"
              :style="{ backgroundColor: getCategoryColor(category), backgroundImage: `url(${product.image})` }"
            >
              <div class="card-header">
                <p>{{ product.category }}</p>
                <span class="menu-icon">...</span>
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
</template>

<style lang="scss" scoped>
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
  height: 350px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;

  &:hover {
    transform: translateY(-10px);
  }
}

.card-header {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  font-size: 14px;
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

.menu-icon {
  cursor: pointer;
  font-size: 18px;
}

footer {
  text-align: center;
  margin-top: 50px;
  p {
    color: #fff;
  }
}
</style>
