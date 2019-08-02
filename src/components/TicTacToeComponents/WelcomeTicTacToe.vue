<template>
  <div class="center">
    <h2>Welcome to Tic-Tac-Toe game board</h2>
    <button
      class="StartButton"
      type="button"
      @click="handleClickStart"
    >Start</button>
  </div>
</template>

<script>
export default {
  props: {
    getPlayerNames: {
      type: Function,
      required: true,
      default: () => null
    }
  },
  methods: {
    playerNamePropmpt (n) {
      return prompt(`What's player ${n} name?`, '')
    },
    handleClickStart () {
      const p1Name = this.playerNamePropmpt(1)
      const p2Name = this.playerNamePropmpt(2)

      if (p1Name && p2Name) {
        this.getPlayerNames(p1Name, p2Name)
      } else {
        alert('Please enter both players names!')
        this.handleClickStart()
      }
    }
  }
}
</script>

<style lang="stylus" scoped>
.StartButton
  margin 0 auto
  padding 10px 20px

.center
  display flex
  flex-direction column
  justify-content center
  text-align center
</style>
