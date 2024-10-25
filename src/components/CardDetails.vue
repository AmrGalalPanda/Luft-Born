<template>
    <div class="overlay" @click="handleOutsideClick">
      <div class="card-details" @click.stop>
        <h2>{{ product.title }}</h2>
        <img :src="product.image" height="150" alt="Product image" class="card-image" />
        <p>Category: {{ product.category }}</p>
        <p>Price: ${{ product.price }}</p>
        <p>Rating: {{ product.rating.rate }} ({{ product.rating.count }} reviews)</p>
        <p>{{ product.description }}</p>
        <button @click="$emit('close')">Close</button>
      </div>
    </div>
  </template>
  <script>
  export default {
    name: "CardDetails",
    props: {
      product: {
        type: Object,
        required: true,
      },
    },
    methods: {
      handleOutsideClick() {
        // Emit 'close' event when clicked outside the card details
        this.$emit('close');
      }
    }
  };
  </script>
   
  <style scoped>
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .card-details {
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 10px;
    background-color: #f9f9f9;
    width: calc(100% - 22vw);
    max-width: 500px;
    margin: 20px auto;
    position: relative;
  }
  
  button {
    background-color: #5747EA;
    color: white;
    border: none;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
  }
  button:hover {
    background-color: #4736c9;
  }
  img {
    width: 50%;
     margin: auto;
    display: block;
  }
  </style>
  