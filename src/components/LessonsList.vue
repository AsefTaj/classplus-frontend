<template>
    <div class="container">
      <h2 class="my-3">Available Lessons</h2>
      <ul class="list-group">
        <li v-for="lesson in lessons" :key="lesson._id" class="list-group-item">
          <strong>{{ lesson.title }}</strong> - {{ lesson.location }} - ${{ lesson.price }} - Spaces: {{ lesson.spaces }}
          <button class="btn btn-primary btn-sm float-end" 
                  @click="addToCart(lesson)" 
                  :disabled="lesson.spaces === 0">
            Add to Cart
          </button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        lessons: [],
      };
    },
    methods: {
      async fetchLessons() {
        const response = await fetch("https://classplus-api.onrender.com/lessons");
        this.lessons = await response.json();
      },
      addToCart(lesson) {
        if (lesson.spaces > 0) {
          lesson.spaces--;
          this.$emit("addToCart", lesson);
        }
      }
    },
    mounted() {
      this.fetchLessons();
    }
  };
  </script>