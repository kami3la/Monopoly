<template>
  <Game />
</template>

<script>
import Game from './components/Game.vue'

export default {
  name: 'App',
  components: {
    Game
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

*::-webkit-scrollbar {
  width: 10px;
}

*::-webkit-scrollbar-track {
  background: transparent;
}

*::-webkit-scrollbar-thumb {
  background-color: #4AAE9B;
  border-radius: 30px;
}
</style>
