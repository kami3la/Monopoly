<template>
  <div class="end-wrapper">
    <div class="end">
      <div class="players">
        <div v-for='player in sortedPlayers' :key='player.id'>
          <div class="player">
            <span>{{ sortedPlayers.indexOf(player) + 1 + '.' }}</span>
            <div class="pawn" :class='"pawn" + player.id'></div>
            <span>{{ player.name }}</span>
            <span>{{ player.points }}</span>
          </div>
        </div>
      </div>
      <button type="button" @click='playAgain'>Zagraj ponownie</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'End',
  props: ['players'],
  data() {
    return {
      sortedPlayers: [],
      again: false
    }
  },
  mounted() {
    this.sortedPlayers = [...this.players].sort((a, b) => (a.points > b.points) ? -1 : 1)
  },
  methods: {
    playAgain() {
      window.location.reload()
    }
  }
}
</script>

<style>
.end-wrapper {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100vw;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 4;
  background-color: rgba(0, 0, 0, 0.7);
}

.end {
  width: 60%;
  height: 60%;
  border-radius: 30px/10px;
  background-color: #fff;
  box-shadow: 2px 2px 20px 1px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
}

.pawn {
  width: 2rem;
  height: 2rem;
  border-radius: 50%;
  margin-right: 1rem;
}

.pawn1 {
  background-color: red;
}

.pawn2 {
  background-color: green;
}

.pawn3 {
  background-color: blue;
}

.pawn4 {
  background-color: purple;
}

.player {
  display: grid;
  grid-template-columns: 1fr 1fr 4fr 1fr;
  margin-bottom: 1rem;
  align-items: center;
}

.players {
  display: flex;
  flex-direction: column;
  height: fit-content;
}

.end button {
  height: fit-content;
  width: fit-content;
  padding: 0.5rem;
  border-radius: 30px/10px;
  cursor: pointer;
}
</style>