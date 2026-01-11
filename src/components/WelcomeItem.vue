<template>
  <div class="welcome">
    <h1>🎮 Game Wishlist</h1>
    <p v-if="message">{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: ""
    }
  },

  mounted() {
    this.message = "🎉 Enjoy your games!"
  }
}
</script>

<style scoped>
.welcome {
  text-align: center;
  margin-bottom: 20px;
  color: #00ffcc;
}
</style>
