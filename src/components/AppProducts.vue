<template>
  <div class="main-block mt-42">
    <div class="product-list">
      <div class="best-sales img-16-xxl img-30-lg img-100 mr-xxl-29 mb-0 mb-21 m-lgm- mt-0 m-auto">
        <div class="profile d-flex pt-0-md p-l0-md pt-35 align-center">
          <div class="img-100-md pl-lg-0 pl-md-42 m-md-auto pl-32 img-30">
            <img class="py-md-32 py-0" src="../assets/images/Bitmap.png" alt="" />
          </div>
          <div class="img-100-md img-70">
            <span class="font-15-mid pl-32 mb-md-10 mb-0 d-block">Report for</span>
            <h2 class="font-40-l later-space pl-32">Best Sales</h2>
          </div>
        </div>
        <ul class="pl-md-32 pl-0 py-lg-0 my-0 py-25 d-md-flex justify-around">
          <li class="pt-17">
            <a href="" :class="{ 'muted': activeTab !== 'Daily', 'active': activeTab === 'Daily' }" @click.prevent="setActiveTab('Daily')">Daily</a>
          </li>
          <li class="pt-17">
            <a href="" :class="{ 'muted': activeTab !== 'Weekly', 'active': activeTab === 'Weekly' }" @click.prevent="setActiveTab('Weekly')">Weekly</a>
          </li>
          <li class="pt-17">
            <a href="" :class="{ 'muted': activeTab !== 'Monthly', 'active': activeTab === 'Monthly' }" @click.prevent="setActiveTab('Monthly')">Monthly</a>
          </li>
        </ul>
      </div>
      <div class="products img-82-xxl img-70-lg img-100">
        <ProductCard :products="sortedProducts" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ProductCard from "./ProductCard.vue";

export default {
  name: "AppProducts",
  components: {
    ProductCard,
  },
  data() {
    return {
      products: [],
      activeTab: 'Weekly',
    };
  },
  computed: {
    sortedProducts() {
      return this.products.slice().sort((a, b) => {
        if (this.activeTab === 'Daily') {
          return b.rating.rate - a.rating.rate;
        } else if (this.activeTab === 'Weekly') {
          return b.price - a.price;
        } else if (this.activeTab === 'Monthly') {
          return b.rating.count - a.rating.count;
        }
      });
    },
  },
  methods: {
    setActiveTab(tab) {
      this.activeTab = tab;
    },
  },
  mounted() {
    axios
      .get("https://fakestoreapi.com/products")
      .then((response) => {
        this.products = response.data.slice(0, 10);
      })
      .catch((error) => console.error("Error fetching products:", error));
  },
};
</script>

<style lang="scss" scoped>
.main-block {
  display: flex;
  width: 100%;
  max-width: 1676px;
  margin: auto;
  flex-wrap: wrap;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  width: 100%;
}

.best-sales {
  height: 518px;
  width: 255px;
  border-radius: 15px;
  background-color: #1c204b;
  overflow: hidden;

  .profile {
    border-radius: 15px;
    padding-bottom: 19px;
  }

  img {
    border-radius: 50%;
    width: 145px;
    height: 145px;
    display: flex;
    margin: auto;
    padding: 32px 0;
  }
}

.products {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.later-space {
  letter-spacing: 12px;
  margin: 0;
}

ul {
  list-style: none;
  padding-left: 0;

  li {
    font-size: 18px;
    font-family: "Rubik-Light";
    font-weight: 400;

    .muted {
      color: #aaa;
      text-decoration: none;
      transition: color 0.3s ease;
    }

    .active {
      color: #fff;
      text-decoration: none;
    }
  }
}

@media screen and (max-width: 992px) {
  .m-md-auto
{
  margin: 0;

}
  .best-sales {
    height: fit-content;
    
    img {
      width: 64px;
      height: 64px;
    }

    width: 100%;
  }

  .products {
    margin: auto;
  }

  .later-space {
    letter-spacing: normal;
  }

  .d-md-flex {
    display: flex;
    flex-wrap: wrap;
  }
}

@media screen and (max-width: 576px) {
  .best-sales,
  .products {
    width: 327px;
  }
}
</style>
