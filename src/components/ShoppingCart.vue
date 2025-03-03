<template>
    <div class="container">
      <h2 class="my-3">Shopping Cart</h2>
      <ul class="list-group" v-if="cart.length > 0">
        <li v-for="(lesson, index) in cart" :key="index" class="list-group-item">
          {{ lesson.title }} - ${{ lesson.price }}
          <button class="btn btn-danger btn-sm float-end" @click="removeFromCart(index)">Remove</button>
        </li>
      </ul>
      <p v-else>No lessons in cart.</p>
  
      <div v-if="cart.length > 0" class="mt-3">
        <input v-model="name" placeholder="Enter your name" class="form-control my-2">
        <input v-model="phone" placeholder="Enter your phone" class="form-control my-2">
        <button class="btn btn-success mt-2" @click="checkout" :disabled="!validInput">Checkout</button>
      </div>
    </div>
  </template>
  

  <script>
export default {
  props: ["cart"], // Receive cart as a prop
  data() {
    return {
      localCart: [...this.cart], // Create a local copy of the cart
      name: "",
      phone: "",
    };
  },
  computed: {
    validInput() {
      return this.name.match(/^[A-Za-z ]+$/) && this.phone.match(/^[0-9]+$/);
    }
  },
  methods: {
    removeFromCart(index) {
      this.localCart.splice(index, 1); // Modify local copy
      this.$emit("update-cart", this.localCart); // Emit updated cart to parent
    },
    async checkout() {
      const response = await fetch("http://localhost:5001/orders", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ user: this.name, phone: this.phone, lessons: this.localCart }),
      });

      const result = await response.json();
      alert(result.message);
      this.localCart = [];
      this.name = "";
      this.phone = "";
      this.$emit("update-cart", this.localCart); // Reset cart in parent
    }
  }
};
</script>