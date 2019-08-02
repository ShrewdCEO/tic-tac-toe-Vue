<template>
  <div>
    <tic-tac-toe
      class="position"
    />
  </div>
</template>

<script>
import TicTacToe from '@/components/TicTacToeComponents/TicTacToe'

export default {
  name: 'home',

  components: {
    TicTacToe
  }
}
</script>

<style lang="stylus" scoped>
.position
  position absolute
  left 50%
  top: 48%
  transform translate(-50%, -50%)
</style>
