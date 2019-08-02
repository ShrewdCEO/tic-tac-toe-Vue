<template>
  <div>
    <welcome-tic-tac-toe
      v-if="welcomeVisibility"
      :get-player-names="handlerGetPlayerNames"
    />
    <board-tic-tac-toe
      v-if="boardVisibility"
      :player-names="playerNames"
      :start-new="handleStartNewGame"
    />
  </div>
</template>

<script>
import WelcomeTicTacToe from '@/components/TicTacToeComponents/WelcomeTicTacToe'
import BoardTicTacToe from '@/components/TicTacToeComponents/BoardTicTacToe'

export default {
  components: {
    WelcomeTicTacToe,
    BoardTicTacToe
  },

  data () {
    return {
      welcomeVisibility: true,
      boardVisibility: false,
      playerNames: {}
    }
  },

  beforeMount () {
    const playerNamesFromLocalStorage = ((storage) => {
      const game = JSON.parse(storage)

      if (game && game.playerNames) {
        return game.playerNames
      }
    })(localStorage.getItem('game'))

    if (playerNamesFromLocalStorage) {
      this.playerNames = playerNamesFromLocalStorage
      this.welcomeVisibility = false
      this.boardVisibility = true
    }
  },

  methods: {
    handlerGetPlayerNames (p1Name, p2Name) {
      this.welcomeVisibility = false
      this.boardVisibility = true

      this.playerNames = {
        p1: p1Name,
        p2: p2Name
      }

      localStorage.setItem('game', JSON.stringify({
        playerNames: this.playerNames
      }))
    },
    handleStartNewGame () {
      this.welcomeVisibility = true
      this.boardVisibility = false
      localStorage.setItem('game', JSON.stringify({}))
    }
  }
}
</script>
