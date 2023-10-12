<template>
  <div class="game-board">
    <!-- Display the message -->
    <div v-if="message" class="message">{{ message }}</div>
    
    <div class="row" v-for="(row, rowIndex) in board" :key="rowIndex">
      <div
        class="cell"
        v-for="(cell, colIndex) in row"
        :key="colIndex"
        @click="makeMove(rowIndex, colIndex)"
      >
        {{ cell }}
      </div>
    </div>

    <!-- Button to reset the game -->
    <button @click="resetGame" v-if="winner || message">Restart Game</button>
  </div>
</template>
<script>
export default {
  data() {	
    return {
      winMatrix: [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
        [0, 4, 8], [2, 4, 6]             // Diagonals
      ],
      board: [
        ['', '', ''],
        ['', '', ''],
        ['', '', ''],
      ],
      currentPlayer: 'X',
      winner: null,
    };
  },
  methods: {
resetGame() {
    this.board = [
      ['', '', ''],
      ['', '', ''],
      ['', '', ''],
    ];
    this.currentPlayer = 'X';
    this.winner = null;
    this.message = '';
  },
    makeMove(row, col) {
  if (!this.board[row][col] && !this.winner) {
    this.board[row][col] = this.currentPlayer;
    if (this.checkWin()) {
      this.winner = this.currentPlayer;
      return; // Exit the method to prevent further moves
    } else {
      this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
    }
  }
},
    checkWin() {
  const board = this.board; // Shorter reference
  for (const [a, b, c] of this.winMatrix) {
    if (
      board[a] === this.currentPlayer &&
      board[b] === this.currentPlayer &&
      board[c] === this.currentPlayer
    ) {
      return true;
    }
  }
  return false;
},
  },
};
</script>

<style scoped>
/* Add styles to set the height of the game board and cells */
.game-board {
  display: inline-block; /* Ensures the game board takes up its content's height */
}

.row {
  display: flex;
}

.cell {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  border: 1px solid #ccc;
  cursor: pointer;
}
</style>

