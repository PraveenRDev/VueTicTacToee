<template>
  <div class="game">
    <div class="game-area">
      <div class="game-title">
        <h1>Tic Tac Toe</h1>
      </div>

      <board :squares="squares" :winner="winner" @click="click" />

      <div class="game-info">
        <p v-if="stepNumber === 0">
          Lets Play! Go &nbsp;<b :class="currentPlayer">{{ currentPlayer }}</b>!
        </p>
        <p v-else-if="!!winner">
          Winner is:&nbsp;
          <b :class="currentPlayer">{{ currentPlayer }}</b>!&nbsp;
          <button @click="restart">Restart Game</button>
        </p>
        <p v-else-if="stepNumber > 8">
          Draw!&nbsp;
          <button @click="restart">Restart Game</button>
        </p>
        <p v-else>
          Turn of &nbsp;
          <b :class="currentPlayer">{{ currentPlayer }}</b>!&nbsp;GO!.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Game',
  components: {
    Board: () => import('./GameBoard')
  },
  data () {
    return {
      squares: Array(9).fill(null),
      stepNumber: 0,
      currentPlayer: 'X',
      winner: null
    }
  },
  methods: {
    isWinner() {
      if (this.winner) return true
      const squares = this.squares
      const matches = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], [0, 3, 6], [1, 4, 7],
        [2, 5, 8], [0, 4, 8], [2, 4, 6]
      ]
      for (let i = 0; i < matches.length; i++) {
        const [a, b, c] = matches[i]
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
          this.winner = [ a, b, c ]
          return true
        }
      }
      return false
    },
    restart() {
      this.squares = Array(9).fill(null)
      this.stepNumber = 0
      this.winner = null
    },
    click (i) {
      if (this.squares[i] || this.winner) return
      this.$set(this.squares, i, this.currentPlayer)
      if (!this.isWinner()) {
        this.stepNumber++
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X'
      }
    }
  }
}
</script>

<style scoped>
.game {
  background-color: rgba(var(--gradient-color-base));
 background-image: linear-gradient(to right, #6a11cb 0%, #2575fc 100%);
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.game-area {
  display: flex;
  flex-flow: column;
}
.game-title {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 0 3vmin;
}
.game-title img {
  margin: 0 12px 0 -20px;
  width: 40px;
  filter: drop-shadow(-1px 1px 0 #0007) drop-shadow(1px -1px 0 #0007) drop-shadow(1px 1px 0 #0007);
}
.game-title h1 {
  margin: 0;
  font-size: 2.25em;
  color: #fff;
  text-shadow: -1px -1px 1px #000b, -1px 1px 1px #000b, 1px -1px 1px #000b, 1px 1px 1px #000b;
}
.game-info {
  margin: 3vmin 0 0;
  padding: 1rem .5rem;
  font-size: 1.25em;
  text-align: center;
  box-shadow: 2.5px 5px 25px #0001, 0 1px 6px #0004;
  text-shadow: 0 0 1px #fff, 0 2px 5px #fff5;
  border-radius: .5rem;
  backdrop-filter: blur(10px);
  background: #fff6;
  background-blend-mode: exclusion;
  background-image: var(--noise-pattern);
  color: #111;
}
.game-info p {
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
.game-info .X,
.game-info .O {
  text-shadow: -1px -1px 0 #000b, -1px 1px 0 #000b, 1px -1px 0 #000b, 1px 1px 0 #000b;
}
.game-info .X {
  color: #000;
}
.game-info .O {
  color: #f43b47;
}
.game-info button {
  text-transform: uppercase;
  font-weight: 600;
  font-size: .75em;
  padding: .5rem 1rem;
  margin: -.5rem 0 -.5rem 1rem;
  border: 2px solid #fff;
  background: #fff5;
  text-shadow: 0 0 1px #fff, 0 2px 5px #fff5;
  color: #111;
  cursor: pointer;
  outline: none;
  transition: all .25s ease;
}
.game-info button:focus,
.game-info button:hover {
  background: #1115;
  border-color: rgba(var(--theme-color));
  box-shadow: 0 0 10px rgba(var(--theme-color), .75);
  color: rgba(var(--theme-color));
  text-shadow: -1px -1px 0 #0007, -1px 1px 0 #0007, 1px -1px 0 #0007, 1px 1px 0 #0007;
}
.game-info button:active {
  background: #1119;
}
</style>