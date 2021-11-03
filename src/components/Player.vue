<template>
  <span class="player" :class='"id" + id'></span>
</template>

<script>
export default {
  name: 'Player',
  props: ['id', 'points', 'i', 'j']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.player {
  width: 20px;
  height: 20px;
  border: solid black 3px;
  border-radius: 10px;
  display: inline-block;
}

.player.id1 {
  background-color: red;
}

.player.id2 {
  background-color: green;
}

.player.id3 {
  background-color: blue;
}

.player.id4 {
  background-color: purple;
}
</style>
