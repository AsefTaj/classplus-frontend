<template>
  <div>
    <LessonsList @addToCart="addToCart"/>
    <ShoppingCart :cart="cart" @update-cart="updateCart"/>
  </div>
</template>

<script>
import LessonsList from './components/LessonsList.vue';
import ShoppingCart from './components/ShoppingCart.vue';

export default {
  components: {
    LessonsList,
    ShoppingCart
  },
  data() {
    return {
      cart: [],
    };
  },
  methods: {
    addToCart(lesson) {
      this.cart.push(lesson);
    },
    updateCart(updatedCart) {
      this.cart = updatedCart;
    }
  }
};
</script>