<template>
  <div class="wishlist">
    <!-- Add Game Form -->
    <div class="add-game">
      <input v-model="newGame.name" placeholder="Game Name" />
      <select v-model="newGame.type">
        <option disabled value="">Select Type</option>
        <option v-for="type in gameTypes" :key="type">{{ type }}</option>
      </select>
      <button @click="addGame">➕ Add Game</button>
    </div>

    <!-- Grouped Games List -->
    <div v-for="type in gameTypes" :key="type" class="type-group">
      <h3>{{ type }}</h3>
      <ul>
        <li
          v-for="game in games.filter(g => g.type === type)"
          :key="game.id"
          class="game-item"
        >
          <!-- Edit Mode -->
          <div v-if="editIndex === game.id" class="edit-mode">
            <input v-model="editGame.name" placeholder="Game Name" />
            <select v-model="editGame.type">
              <option v-for="t in gameTypes" :key="t">{{ t }}</option>
            </select>
            <button @click="saveEdit(game.id)">💾 Save</button>
            <button @click="cancelEdit">❌ Cancel</button>
          </div>

          <!-- Display Mode -->
          <div v-else class="display-mode">
            <span>{{ game.name }}</span> 
            <span class="type-label">{{ game.type }}</span>
            <button @click="startEdit(game)">✏️ Edit</button>
            <button @click="deleteGame(game.id)">🗑️ Delete</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "GameWishlist",
  data() {
    return {
      newGame: { name: "", type: "" },
      editGame: { name: "", type: "" },
      editIndex: null,
      gameTypes: ["Action", "Adventure", "RPG", "Puzzle", "Sandbox", "Sports", "Simulation"],
      games: [],
      nextId: 1, // unique id for each game
    };
  },
  methods: {
    addGame() {
      if (this.newGame.name.trim() && this.newGame.type) {
        const game = { ...this.newGame, id: this.nextId++ };
        this.games.push(game);
        this.newGame.name = "";
        this.newGame.type = "";
        this.saveGames();
      }
    },
    deleteGame(id) {
      this.games = this.games.filter(g => g.id !== id);
      this.saveGames();
    },
    startEdit(game) {
      this.editIndex = game.id;
      this.editGame = { ...game };
    },
    saveEdit(id) {
      const index = this.games.findIndex(g => g.id === id);
      if (index !== -1) {
        this.games[index] = { ...this.editGame, id };
        this.editIndex = null;
        this.editGame = { name: "", type: "" };
        this.saveGames();
      }
    },
    cancelEdit() {
      this.editIndex = null;
      this.editGame = { name: "", type: "" };
    },
    saveGames() {
      localStorage.setItem("games", JSON.stringify(this.games));
    },
    loadGames() {
      const saved = localStorage.getItem("games");
      if (saved) {
        this.games = JSON.parse(saved);
        if (this.games.length) {
          this.nextId = Math.max(...this.games.map(g => g.id)) + 1;
        }
      } else {
        const defaultGames = [
          { name: "The Legend of Zelda", type: "Adventure" },
          { name: "God of War Ragnarok", type: "Action" },
          { name: "Minecraft", type: "Sandbox" },
          { name: "Cyberpunk 2077", type: "RPG" },
          { name: "Tetris", type: "Puzzle" },
        ];
        this.games = defaultGames.map(g => ({ ...g, id: this.nextId++ }));
        this.saveGames();
      }
    },
  },
  mounted() {
    this.loadGames();
  },
};
</script>

<style scoped>
.wishlist {
  width: 450px;
  margin: 0 auto;
  text-align: center;
  color: #fff;
}

/* Add Game Form */
.add-game {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 2rem;
}

.add-game input,
.add-game select {
  padding: 0.4rem;
  border-radius: 5px;
  border: none;
  outline: none;
  background-color: #2c2c2c;
  color: #fff;
}

.add-game button {
  padding: 0.4rem 0.8rem;
  background: #4caf50;
  color: #fff;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}

/* Grouped Games */
.type-group {
  margin-bottom: 1.5rem;
}

.type-group h3 {
  text-align: left;
  border-bottom: 1px solid #555;
  padding-bottom: 0.2rem;
  margin-bottom: 0.5rem;
}

/* Game Items */
.game-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.5rem;
  padding: 0.5rem;
  background-color: #1e1e1e;
  border-radius: 6px;
}

.game-item input,
.game-item select {
  padding: 0.3rem;
  border-radius: 4px;
  border: none;
  outline: none;
  background-color: #2c2c2c;
  color: #fff;
}

/* Buttons */
.game-item button {
  background: #e53935;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  padding: 0.3rem 0.6rem;
}

.edit-mode button:first-child {
  background: #4caf50;
  margin-right: 0.3rem;
}

.display-mode .type-label {
  margin-left: 0.5rem;
  font-size: 0.9rem;
  opacity: 0.7;
}
</style>
