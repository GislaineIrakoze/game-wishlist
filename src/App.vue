<template>
  <div class="app">
    <header>
      <h1>🎮 Game Wishlist</h1>
    </header>

    <p class="welcome">{{ welcomeMessage }}</p>

    <GameWishlist />
  </div>
</template>

<script>
import GameWishlist from "./components/Gamewishlist.vue";

export default {
  name: "App",
  components: { GameWishlist },
  data() {
    return {
      welcomeMessage: "",
    };
  },
  mounted() {
    this.welcomeMessage = "Enjoy your games! 🎉";
  },
};
</script>

<style>
/* Center everything vertically and horizontally */
.app {
  font-family: Arial, sans-serif;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #121212;
  color: #fff;
  padding: 2rem;
}

header {
  margin-bottom: 1rem;
}

.welcome {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  text-align: center;
}
</style>
