<template>
  <header class="header d-flex m-auto">
    <a a href="#/home" class="my-0 m-auto d-block mr-md-35">
      <img :class="{ active: isActive('/home') }" @click="setActive('/home')" width="200" height="50" src="../assets/images/logo.svg" alt="Logo" class="logo" />
    </a>
    <nav class="m-auto">
      <ul>
        <li><a href="#/home" :class="{ active: isActive('/home') }" @click="setActive('/home')">Home</a></li>
        <li><a href="#/about" :class="{ active: isActive('/about') }" @click="setActive('/about')">About</a></li>
        <li><a href="#/AppProducts" :class="{ active: isActive('/AppProducts') }" @click="setActive('/AppProducts')">Best Sales</a></li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',
  data() {
    return {
      activeLink: ''
    };
  },
  methods: {
    setActive(link) {
      this.activeLink = link;
    },
    isActive(link) {
      return this.activeLink === link;
    }
  },
  mounted() {
    // Set default active link based on current URL
    this.activeLink = window.location.hash || '/AppProducts';
  }
}
</script>

<style scoped lang="scss">
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  background-color: #3a3a3a;
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease-in-out;

  .logo {
    transition: transform 0.4s ease, opacity 0.4s;
    &:hover {
      transform: scale(1.05) rotate(-5deg);
      opacity: 0.9;
    }
  }

  nav ul {
    list-style: none;
    display: flex;
    gap: 25px;

    li a {
      text-decoration: none;
      color: #ffffff;
      font-weight: 600;
      position: relative;
      transition: color 0.3s ease-in-out;

      &::after {
        content: '';
        position: absolute;
        width: 0;
        height: 2px;
        background-color: #ff9900;
        left: 0;
        bottom: -5px;
        transition: width 0.3s ease-in-out;
      }

      &:hover,
      &.active {  
        color: #ff9900;
      }

      &:hover::after,
      &.active::after {
        width: 100%;
      }
    }
  }
}
</style>
