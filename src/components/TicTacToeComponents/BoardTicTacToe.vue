<template>
  <div class="center">
    <h2>Tic-Tac-Toe</h2>
    <div class="PlayersTicTacToe">
      <div>{{playerNames.p1}}: {{scores.p1}}</div>
      <div>{{playerNames.p2}}: {{scores.p2}}</div>
    </div>
    <div class="BoardTicTacToe">
      <div
        class="BoardTicTacToe-button"
        v-for="(sign, k) in board"
        :key="k"
        @click="handleClickButton(k)"
        v-text="sign"
      />
    </div>
    <h3>It's your move {{playerNameByMove}}!</h3>

    <div class="ActionsTicTacToe">
      <button
        type="button"
        @click="handleClickRestart"
      >Restart</button>

      <button
        type="button"
        @click="handleClickReset"
      >Reset</button>

      <button
        type="button"
        @click="handleClickNew"
      >New</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    playerNames: {
      type: Object,
      required: true,
      default: () => {}
    },
    startNew: {
      type: Function,
      required: true,
      default: () => null
    }
  },

  computed: {
    playerNameByMove () {
      return this.playerNames[`p${this.moves.p1 ? 1 : 2}`]
    }
  },

  data () {
    return {
      board: [],
      moves: {
        p1: false,
        p2: false
      },
      scores: {
        p1: 0,
        p2: 0
      }
    }
  },

  beforeMount () {
    const game = this.getFromLocalStorage()
    if (game && game.board && game.moves && game.scores) {
      this.board = game.board
      this.moves = game.moves
      this.scores = game.scores
    } else {
      this.initBoard()
    }
  },

  methods: {
    getRandomBetween  (min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min
    },

    getFromLocalStorage () {
      const gameLocalStorage = localStorage.getItem('game')
      const game = JSON.parse(gameLocalStorage)
      return game
    },

    generateArray (n) {
      return Array
        .apply(null, { length: n })
    },

    initBoard () {
      this.board = this.generateArray(9)
      const random = this.getRandomBetween(0, 1)
      this.moves.p1 = random === 0
      this.moves.p2 = random === 1
    },

    changeMove () {
      if (this.moves.p1) {
        this.moves.p1 = false
        this.moves.p2 = true
      } else if (this.moves.p2) {
        this.moves.p1 = true
        this.moves.p2 = false
      }
    },

    checkFinishedStatus () {
      const check = (n1, n2, n3) =>
        (this.board[n1] === 'X' || this.board[n1] === '0') && (this.board[n1] === this.board[n2] && this.board[n2] === this.board[n3])

      const winnerIfAny = [
        // horizontal
        check(0, 1, 2),
        check(3, 4, 5),
        check(6, 7, 8),

        // vertical
        check(0, 3, 6),
        check(1, 4, 7),
        check(2, 5, 8),

        // cross
        check(0, 4, 8),
        check(2, 4, 6)
      ]

      const someoneIsWinner = winnerIfAny
        .find((i) => i === true)

      const isDraw = (() => {
        return this.board
          .filter(i => i === 'X' || i === '0')
          .length === 9
      })()

      if (someoneIsWinner) {
        return 'winner'
      } else if (isDraw) {
        return 'draw'
      } else {

      }
    },

    checkWinner () {
      if (this.checkFinishedStatus() === 'winner') {
        const pN = this.moves.p1
          ? 1
          : 2

        ++this.scores[`p${pN}`]

        alert(`Winner is ${this.playerNameByMove}!`)

        this.initBoard()
      } else if (this.checkFinishedStatus() === 'draw') {
        alert('It\'s draw!')
        this.initBoard()
      } else {
        this.changeMove()
      }
    },

    saveInLocalStorage () {
      localStorage.setItem('game', JSON.stringify({
        board: this.board,
        moves: this.moves,
        scores: this.scores,
        playerNames: this.playerNames
      }))
    },

    handleClickButton (k) {
      const allowable = !this.board[k]

      if (!allowable) {
        return false
      }

      const sign = this.moves.p1 ? 'X' : '0'
      this.$set(this.board, k, sign)

      requestAnimationFrame(() => {
        this.checkWinner()
        this.saveInLocalStorage()
      })
    },

    handleClickRestart () {
      this.initBoard()
      this.saveInLocalStorage()
    },

    handleClickReset () {
      this.initBoard()
      this.scores = {
        p1: 0,
        p2: 0
      }
      this.saveInLocalStorage()
    },

    handleClickNew () {
      this.startNew()
    }
  }
}
</script>

<style lang="stylus" scoped>
$width = 300px
$height = 300px

$borderColor = black
$buttonBorderWidth = 2px

.BoardTicTacToe
  border 1px solid $borderColor
  width $width
  height $height
  display flex
  flex-wrap wrap
  &-button
    border $buttonBorderWidth solid $borderColor
    width ($width / 3 - $buttonBorderWidth * 2)
    height ($width / 3 - $buttonBorderWidth * 2)

    display flex
    justify-content center
    align-items center

    font-size 50px
    cursor pointer

.PlayersTicTacToe
  display flex
  justify-content space-between
  margin 10px 0

.ActionsTicTacToe
  display flex
  justify-content space-between

.center
  text-align center
</style>
