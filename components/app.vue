<template>
  <div class="flex flex-col items-center justify-center min-h-screen bg-gray-100">
    <div v-if="!player1Name || !player2Name" class="max-w-sm mx-auto">
      <h1 class="text-3xl font-semibold mb-4">Welcome to register Player names</h1>
      <form @submit.prevent="registerPlayer" class="bg-white bg-opacity-50 p-6 rounded shadow-lg max-w-md mx-auto mt-10">
        <div class="mb-5">
          <label for="player1" class="block mb-2 text-sm font-medium text-gray-900 dark:text-blue">Player 1 Name</label>
          <input v-model="inputName1" id="player1" type="text" class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 dark:shadow-sm-light" placeholder="Enter player 1 name" required />
        </div>
        <div class="mb-5">
          <label for="player2" class="block mb-2 text-sm font-medium text-gray-900 dark:text-">Player 2 Name</label>
          <input v-model="inputName2" id="player2" type="text" class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 dark:shadow-sm-light" placeholder="Enter player 2 name" required />
        </div>
        <button type="submit" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Register</button>
      </form>
    </div>
    <div v-else>
      <div class="mb-4 text-3xl font-bold text-center">TIC TAC TOE</div>
      <div class="mb-4 text-center">
        <span class="font-semibold">{{ player1Name }}</span> (X) vs <span class="font-semibold">{{ player2Name }}</span> (O)
      </div>
      <!-- Display current user turn with the name -->
      <div class="mb-4 text-center">
        <span class="font-semibold">{{ currentPlayer === 'X' ? player1Name : player2Name }}</span>'s turn
      </div>
      <div class="grid grid-cols-5 gap-2">
        <div
      v-for="(cell, index) in board"
      :key="index"
      class="w-16 h-16 border-2 border-gray-600 flex items-center justify-center cursor-pointer text-3xl font-bold rounded-lg " 
      :class="{ 'bg-green-200': cell === 'X', 'bg-blue-200': cell === 'O' }"
      @click="makeMove(index)"
    >
     {{ cell }}
      </div>

      </div>
      <div v-if="gameOver" class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-75">
        <div v-if="gameOver" class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-75">
  <div class="bg-white p-8 rounded-lg flex flex-col items-center justify-center">
    <h2 class="text-2xl font-semibold mb-4">
      Game Over! 
      <template v-if="winner">
        {{ winner }} wins!
      </template>
      <template v-else>
        No one wins
      </template>
    </h2>
    <button @click="resetGame" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mt-4">Reset Game</button>
  </div>
</div>

</div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      inputName1: '',
      inputName2: '',
      player1Name: null,
      player2Name: null,
      board: Array(25).fill(null),
      currentPlayer: 'X',
      gameOver: false,
      winner: null, // Track the winner separately
    }
  },
  methods: {
    registerPlayer() {
      this.player1Name = this.inputName1;
      this.player2Name = this.inputName2;
    },
    checkWinner() {
      const lines = [
        [0, 1, 2, 3],
        [4, 5, 6, 7, 8, 9],
        [10, 11, 12, 13, 14],
        [15, 16, 17, 18, 19],
        [20, 21, 22, 23, 24],
        [0, 5, 10, 15, 20],
        [1, 6, 11, 16, 21],
        [2, 7, 12, 17, 22],
        [3, 8, 13, 18, 23],
        [4, 9, 14, 19, 24],
        [0, 6, 12, 18, 24],
        [4, 8, 12, 16, 20]
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c, d, e] = lines[i];
        if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c] && this.board[a] === this.board[d] && this.board[a] === this.board[e]) {
          this.gameOver = true;
          this.winner = this.board[a] === 'X' ? this.player1Name : this.player2Name;
          return this.winner;
        }
      }
      return null;
    },
    makeMove(index) {
  if (this.board[index] === null && !this.gameOver) { // Check if the game is not over
    this.board[index] = this.currentPlayer;
    this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
    const winner = this.checkWinner();
    if (winner) {
      this.$refs.winnerModal.show(); // Show the winner modal if a winner is detected
    } else if (!this.board.includes(null)) {
      this.$refs.drawModal.show(); // Show the draw modal if there's a draw
    }
  }
},

    resetGame() {
      this.gameOver = false;
      this.board = Array(25).fill(null);
      this.currentPlayer = 'X';
      this.winner = null;
    }
  }
}
</script>

