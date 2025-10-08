<template>
  <div class="container">
    <h1>Tic Tac Toe</h1>

    <div class="board">
      <div
        v-for="(cell, index) in cells"
        :key="index"
        class="cell"
        @click="handleClick(index)"
      >
        {{ cell }}
      </div>
    </div>

    <p class="status">{{ statusMessage }}</p>

    <button class="reset-btn" @click="resetGame">Reset Game</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      size: 3, // can even make it dynamic for 4x4 or 5x5
      cells: Array(9).fill(""),
      currentPlayer: "X",
      gameActive: true,
    };
  },
  computed: {
    statusMessage() {
      if (!this.gameActive) {
        if (this.checkWin()) return `${this.currentPlayer} Wins 🎉`;
        if (!this.cells.includes("")) return "It's a Tie!";
      }
      return `Current Turn: ${this.currentPlayer}`;
    },
  },
  methods: {
    handleClick(index) {
      if (!this.gameActive || this.cells[index]) return;

      this.cells[index] = this.currentPlayer;

      if (this.checkWin() || !this.cells.includes("")) {
        this.gameActive = false;
      } else {
        this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
      }
    },

    // ✅ Check rows, columns, and diagonals dynamically
    checkWin() {
      const { size, cells } = this;
      const player = this.currentPlayer;

      // Check rows
      for (let r = 0; r < size; r++) {
        let rowStart = r * size;
        if (
          cells[rowStart] &&
          cells.slice(rowStart, rowStart + size).every((c) => c === player)
        )
          return true;
      }

      // Check columns
      for (let c = 0; c < size; c++) {
        let col = [];
        for (let r = 0; r < size; r++) {
          col.push(cells[r * size + c]);
        }
        if (col.every((v) => v === player && v !== "")) return true;
      }

      // Check main diagonal
      let mainDiag = [];
      for (let i = 0; i < size; i++) {
        mainDiag.push(cells[i * size + i]);
      }
      if (mainDiag.every((v) => v === player && v !== "")) return true;

      // Check anti-diagonal
      let antiDiag = [];
      for (let i = 0; i < size; i++) {
        antiDiag.push(cells[i * size + (size - 1 - i)]);
      }
      if (antiDiag.every((v) => v === player && v !== "")) return true;

      return false;
    },

    resetGame() {
      this.cells = Array(this.size * this.size).fill("");
      this.currentPlayer = "X";
      this.gameActive = true;
    },
  },
};
</script>

<style scoped>
.container {
  font-family: Arial, sans-serif;
  text-align: center;
  padding: 2rem;
  background-color: #f7f9fc;
  min-height: 100vh;
}

h1 {
  margin-bottom: 20px;
  color: #333;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 5px;
  justify-content: center;
  margin: 0 auto;
}

.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  font-weight: bold;
  background: #fff;
  border: 2px solid #333;
  cursor: pointer;
  transition: background 0.3s ease;
}

.cell:hover {
  background: #e0e7ff;
}

.status {
  margin-top: 20px;
  font-weight: 500;
  color: #444;
}

.reset-btn {
  margin-top: 15px;
  padding: 8px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.reset-btn:hover {
  background-color: #0056b3;
}
</style>
