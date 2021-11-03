<template>
  <td v-if="buildingCard">
    <div class="building-card">
      <div class="header" :class='"-set" + id'>{{ name }}</div>
      <div class="body">
        <div>{{ description }}</div>
        <div><Player v-for='playerId in playersIds' :id='playerId' :key='playerId' /></div>
      </div>
    </div>
  </td>
  <td v-else-if="image" class="corner image" :style="defaultImage">
    <Player v-for='playerId in playersIds' :id='playerId' :key='playerId' />
  </td>
  <td v-else class="corner">
    <Player v-for='playerId in playersIds' :id='playerId' :key='playerId' />
  </td>
</template>

<script>
import Player from './Player'

export default {
  name: 'Cell',
  props: ['id', 'name', 'description', 'correctAnswer', 'points', 'i', 'j', 'players', 'image'],
  components: {
    Player
  },
  computed: {
    buildingCard() {
      return this.id
    },
    playersIds() {
      let ids = []
      this.players.forEach(player => {
        if (player.i === this.i && player.j === this.j) {
          ids.push(player.id);
        }
      });
      return ids;
    },
    defaultImage() {
      return `background-image: url(${this.image})`
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.-set1 {
  background-color: #DF3FEE;
}

.-set2 {
  background-color: #7F211B;
}

.-set3 {
  background-color: #C70C13;
}

.-set4 {
  background-color: #D7E60C;
}

.-set5 {
  background-color: #181798;
}

.-set6 {
  background-color: #F8A9FC;
}

.-set7 {
  background-color: #35EBD4;
}

.-set8 {
  background-color: #31FA1D;
}

.-set9 {
  background-color: #008080;
}

.building-card {
  height: 100%;
  width: 100%;
  display: grid;
  grid-template-rows: 1fr 2.5fr;
  position: relative;
  right: 1px;
  bottom: 1px;
}

.header,
.body {
  height: calc(100% + 2px);
  width: calc(100% + 2px);
  z-index: -1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.body,
.corner {
  background-color: #fff;
}

.image {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: 90% 60%;
}

.body {
  display: flex;
  flex-direction: column;
}
</style>
