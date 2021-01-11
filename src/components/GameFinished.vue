<template>
  <p>Game is finished, you {{ wonOrLost }}</p>
</template>
<script>
  export default {
    name: 'GameFinished',
    props: {
      isWon: Boolean
    },
    computed: {
      wonOrLost: function() {
        return this.isWon ? "Won" : "Lost";
    }
  }
}
</script>
