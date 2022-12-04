<template>
  <div>
    <h1>Tic Tac Toe</h1>
    <div class="game">
      <div class="board">
        <div
        v-for="(square, index) in squares" 
        :key="index" 
        :class="{ square: true, x: square === 'X', o: square === 'O' }"
        @click="handleClick(index)">
        </div>
      </div>
      <div v-if="winner" class="controls">
        {{ winner }} has won the game!
      </div>
      <div class="controls">
        <button @click="resetGame">Reset Game</button>
      </div>
    </div>
  </div>
</template>

<script>
function detectWinner(board) {
  const winningLines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (const line of winningLines) {
    const [a, b, c] = line;
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }

  return null;
}

export default {
  name: 'TicTacToe',
  data() {
    return {
      squares: new Array(9).fill(null),
      xIsNext: true,
    };
  },
  computed: {
    winner() {
      return detectWinner(this.squares);
    },
  },
  methods: {
    handleClick(index) {
      // Don't allow moves on a filled square or when the game is over
      if (this.calculateWinner() || this.squares[index]) {
        return;
      }
      // Place either an 'X' or an 'O' in the clicked square
      this.squares.splice(index, 1, this.xIsNext ? 'X' : 'O');
      // Switch turns
      this.xIsNext = !this.xIsNext;
    },
    resetGame() {
      this.squares = new Array(9).fill(null);
      this.xIsNext = true;
    },
    calculateWinner() {
      // Get all possible winning combinations
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      // Loop through each winning combination and check if all the squares are filled with the same value
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (this.squares[a] && this.squares[a] === this.squares[b] && this.squares[a] === this.squares[c]) {
          // If a winning combination is found, return the winner ('X' or 'O')
          return this.squares[a];
        }
      }
      // If no winning combination is found, return null
      return null;
    },
  },
};
</script>

<style>
.game {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.board {
  display: grid;
  /* create a 3x3 grid for the game board */
  grid-template-columns: repeat(3, 1fr);
  /* add a gap between the squares of the game board */
  grid-gap: 10px;
  width: 300px;
  height: 300px;

  /* add some border to the board to make the lines visible */
  border: 1px solid black;
}
.square {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 3em;

  /* add a black border to each square of the game board */
  border: 1px solid black;
}

/* use the :before and :after pseudo-elements to add the X and O symbols */
.square:before,
.square:after {
  content: '';
}

/* use a circle emoji for the O symbol */
.square.o:before {
  content: '⭕';
}

/* use an X emoji for the X symbol */
.square.x:before {
  content: '❌';
}
.controls {
  margin-top: 20px;
}
</style>