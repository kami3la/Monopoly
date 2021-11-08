<template>
  <div class="start" :style="game ? {'display': 'none'} : {'display': 'auto'}">
    <div class="options">
      <div class="options-modal">
        <button type="button" class="button-modal" @click="showModal">?</button>
        <Modal v-show="isModalVisible" @close="closeModal" />
      </div>
      <div class="players">
        <div class="player player-1">
          <div class="pawn pawn1"></div>
          <input type="text" class="label" placeholder="Gracz 1" required maxlength="9" pattern="[A-Za-z0-9]+" id="player-1">
        </div>
        <div class="player player-2">
          <div class="pawn pawn2"></div>
          <input type="text" class="label" placeholder="Gracz 2" required maxlength="9" pattern="[A-Za-z0-9]+" id="player-2">
        </div>
        <div class="player player-3">
          <div class="pawn pawn3"></div>
          <input type="text" class="label" placeholder="Gracz 3" required maxlength="9" pattern="[A-Za-z0-9]+" id="player-3">
        </div>
        <div class="player player-4">
          <div class="pawn pawn4"></div>
          <input type="text" class="label" placeholder="Gracz 4" required maxlength="9" pattern="[A-Za-z0-9]+" id="player-4">
        </div>
      </div>
      <button class="start-button" type="submit" @click="submit">Graj</button>
    </div>
  </div>
  <Game :players='players' v-if='game' />
</template>

<script>
import Modal from './Modal.vue'
import Game from './Game.vue'

export default {
  name: 'Start',
  data() {
    return{
      isModalVisible: false,
      players: [],
      game: false
    }
  },
  components: {
    Modal,
    Game
  },
  methods: {
    showModal() {
      this.isModalVisible = true
    },
    closeModal() {
      this.isModalVisible = false
    },
    submit() {
      const input1 = document.getElementById('player-1');
      const input2 = document.getElementById('player-2');
      const input3 = document.getElementById('player-3');
      const input4 = document.getElementById('player-4');

      const players = [input1, input2, input3, input4];

      for (let i = 0; i < players.length; i++) {
        if (players[i].value !== '') {
          this.players.push({id: i+1, name: players[i].value});
        }
      }

      this.game = true;
    },
    playAgain() {
      this.game = false;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.start {
  width: 100vw;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 3;
  background-color: rgba(0, 0, 0, 0.3);
}

.options {
  width: 60vw;
  height: 60vh;
  border-radius: 30px/10px;
  background-color: #fff;
  box-shadow: 2px 2px 20px 1px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.options-modal {
  width: 100%;
  display: flex;
  justify-content: flex-end;
}

.button-modal {
  height: fit-content;
  width: fit-content;
  padding: 0.5rem;
  border-radius: 30px/10px;
  margin: 1rem;
  cursor: pointer;
}

.start-button {
  height: fit-content;
  width: fit-content;
  padding: 0.5rem 3rem;
  border-radius: 30px/10px;
  margin: 1rem;
  cursor: pointer;
}

.players {
  width: 80%;
  height: 60%;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  justify-items: center;
  align-items: flex-start;
}

.player {
  display: flex;
  flex-direction: row;
  height: fit-content;
}

.label {
  padding: 0.2rem 0.5rem;
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
</style>
