<template>
  <div class="tic-tac-toe">
    <h1>Tic-Tac-Toe</h1>
    
    <!-- Display the message and restart button -->
    <div v-if="winner" class="message">{{ winner }} wins! <button @click="resetGame">Restart</button></div>
    <div v-else-if="isDraw" class="message">It's a draw! <button @click="resetGame">Restart</button></div>
    <div v-else>
      <p>Current player: {{ currentPlayer }}</p>
    </div>
    
    <!-- The game board -->
    <div class="board">
      <div
        class="cell"
        v-for="(cell, index) in board"
        :key="index"
        @click="makeMove(index)"
        :class="{ 'cell-disabled': cell || winner }"
      >
        {{ cell }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(''), // 3x3 board represented as a 1D array
      currentPlayer: 'X',
      winner: null,
    };
  },
  computed: {
    isDraw() {
      return !this.board.includes('') && !this.winner;
    },
  },
  methods: {
    makeMove(index) {
      if (!this.board[index] && !this.winner) {
        this.board[index] = this.currentPlayer;
        if (this.checkWin()) {
          this.winner = this.currentPlayer;
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
      }
    },
    checkWin() {
      const winConditions = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
        [0, 4, 8], [2, 4, 6]             // Diagonals
      ];

      for (const condition of winConditions) {
        const [a, b, c] = condition;
        if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c]) {
          return true;
        }
      }
      return false;
    },
    resetGame() {
      this.board = Array(9).fill('');
      this.currentPlayer = 'X';
      this.winner = null;
    },
  },
};
</script>

<style scoped>
.tic-tac-toe {
  text-align: center;
  font-family: Arial, sans-serif;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 5px;
  margin: 20px auto;
  max-width: 300px;
}

.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100px;
  height: 100px;
  background-color: #eee;
  font-size: 24px;
  cursor: pointer;
}

.cell-disabled {
  cursor: not-allowed;
}

.message {
  margin: 20px 0;
  font-size: 18px;
}

button {
  font-size: 18px;
  padding: 5px 10px;
  margin-left: 10px;
  background-color: #007BFF;
  color: #fff;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>

