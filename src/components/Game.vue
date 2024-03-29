<template>
  <table class="desk">
    <tr v-for='(row, i) in cells' :key='[row, i]'>
      <template v-for='(cell, j) in row' :key='[cell, j]'>
        <Cell class="cell"
          v-if='cell'
          :i='i'
          :j='j'
          :players='gamePlayers'
          :description='cell.description'
          :id='cell.id'
          :points='cell.points'
          :name='cell.name'
          :image='cell.image'
          :correctAnswer='cell.correctAnswer'
        />
        <td class="options-wrapper" v-else-if='i==1 && j==1'>
          <div class="options" :style="move ? {'display': 'none'} : {'display': 'auto'}">
            <button type="button" class="button-info" @click='showModal'>?</button>
            <Modal v-show="isModalVisible" @close='closeModal' />
            <button type="button" class="button-close" @click='ifEndGameOpen'>x</button>
            <div v-show="isEndModalVisible" class="modal-ifEnd-wrapper" @click='ifEndGameClose'>
              <div class="modal-ifEnd" @click.stop="">
                <div class="modal-ifEnd-header">
                  <slot name="header"><h1>Chcesz zakończyć rozgrywkę?</h1></slot>
                </div>
                <div class="modal-ifEnd-body">
                  <slot name="body">
                    <button type="button" class="modal-ifEnd-button" @click='end'>Tak</button>
                    <button type="button" class="modal-ifEnd-button" @click='ifEndGameClose'>Nie</button>
                    <End :players='gamePlayers' v-show='endGame' />
                  </slot>
                </div>
              </div>
            </div>
          </div>
        </td>
        <td class="inner" v-else-if='i==2 && j == 1'>
          <h1 class="title">MATMOPOLY</h1>
          <table class="players-info">
            <tr>
              <th v-for='player in gamePlayers' :key='player.id'>
                <span v-show='player.id === currentPlayer.id' class="turn">↓</span>
              </th>
            </tr>
            <tr>
              <th v-for='player in gamePlayers' :key='player.id'>
                <Player :id='player.id' :name='player.name' />
              </th>
            </tr>
            <tr>
              <td v-for='player in gamePlayers' :key='player.name' class="table-name">{{ player.name }}</td>
            </tr>
            <tr>
              <td v-for='player in gamePlayers' :key='player.id' class="table-points">{{ player.points }}</td>
            </tr>
          </table>
          <div class="actions">
            <div class="actions-role">
              <div class="dices">
                <Dice class="dice" v-for='(dice, i) in dices' :value='dice' :key="i" />
              </div>
              <button @click="roll" ref="buttonRole">Rzuć kośćmi</button>
            </div>
            <div class="actions-input" ref="inputWrapper">
              <input placeholder="Wpisz odpowiedź..." id="input" @keyup.enter="answer" />
              <button @click="answer" type="submit" id="button">Wyślij</button>
            </div>
            <span class="actions-text1" ref="text1">Dobrze!</span>
            <span class="actions-text2" ref="text2">Źle!</span>
          </div>
        </td>
      </template>
    </tr>
  </table>
</template>

<script>
import Cell from './Cell'
import Player from './Player'
import Dice from './Dice'
import Modal from './Modal'
import End from './End'
import { Challenges } from '../constants/Challenges'

const randomInteger = function (min, max) {
  return Math.floor(min + Math.random() * (max + 1 - min))
}
const randomDice = () => randomInteger(1, 6)

class DesktopCell {
  constructor(name, moveDir, image) {
    this.name = name
    this.moveDir = moveDir
    this.image = image
  }

  onStepOverStart() {
    // Does nothing by default
  }
}

class BuildingCell extends DesktopCell {
  constructor(moveDir, id, description, correctAnswer, points, name) {
    super(moveDir)
    this.moveDir = moveDir
    this.id = id
    this.description = description
    this.correctAnswer = correctAnswer
    this.points = points
    this.name = name
  }
}

class ChallengeCell extends DesktopCell {
  constructor(moveDir) {
    super('challenge', moveDir, 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867702/wykrzyknik_iapdmc.png')
  }
}

class StartCell extends DesktopCell {
  constructor(moveDir) {
    super('start', moveDir, 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867923/start_drivuq.png');
  }

  onStepOverStart(player) {
    return player.points += 20;
  }
}

class DesktopPlayer {
  constructor(id, name) {
    this.i = 5
    this.j = 13
    this.points = 0
    this.id = id
    this.name = name
  }
}

const parkingCell = new DesktopCell('parking', 'up', '')
const jailCell = new DesktopCell('jail', 'right', 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867760/wiezienie_wtpaoa.png')
const toJailCell = new DesktopCell('toJail', 'down', 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867761/dowiezenia_bwkp0w.png')
const startCell = new StartCell('left', '')

let cells = new Array(6);

for (let i = 0; i < 6; i++) {
  cells[i] = new Array(14);
}

cells[5][13] = startCell
cells[5][12] = new BuildingCell('left', 1, '2 · 3', 6, 2, '+2 pkt')
cells[5][11] = new ChallengeCell('left')
cells[5][10] = new BuildingCell('left', 1, '3 · 3', 9, 2, '+2 pkt')
cells[5][9] = new BuildingCell('left', 2, '4 · 3', 12, 3, '+3 pkt')
cells[5][8] = new ChallengeCell('left')
cells[5][7] = new BuildingCell('left', 2, '4 · 4', 16, 3, '+3 pkt')
cells[5][6] = new BuildingCell('left', 2, '5 · 4', 20, 3, '+3 pkt')
cells[5][5] = new ChallengeCell('left')
cells[5][4] = new BuildingCell('left', 3, '5 · 5', 25, 4, '+4 pkt')
cells[5][3] = new BuildingCell('left', 3, '6 · 4', 24, 4, '+4 pkt')
cells[5][2] = new BuildingCell('left', 3, '3 · 7', 21, 4, '+4 pkt')
cells[5][1] = new BuildingCell('left', 4, '7 · 6', 42, 5, '+5 pkt')
cells[5][0] = parkingCell
cells[4][0] = new BuildingCell('up', 4, '5 · 7', 35, 5, '+5 pkt')
cells[3][0] = new BuildingCell('up', 4, '4 · 9', 36, 5, '+5 pkt')
cells[2][0] = new ChallengeCell('up')
cells[1][0] = new BuildingCell('up', 5, '5 · 9', 45, 6, '+6 pkt')
cells[0][0] = jailCell
cells[0][1] = new BuildingCell('right', 5, '6 · 8', 48, 6, '+6 pkt')
cells[0][2] = new BuildingCell('right', 5, '7 · 7', 49, 6, '+6 pkt')
cells[0][3] = new BuildingCell('right', 6, '8 · 7', 56, 7, '+7 pkt')
cells[0][4] = new BuildingCell('right', 6, '5 · 11', 55, 7, '+7 pkt')
cells[0][5] = new BuildingCell('right', 6, '6 · 9', 54, 7, '+7 pkt')
cells[0][6] = new BuildingCell('right', 7, '7 · 9', 63, 8, '+8 pkt')
cells[0][7] = new BuildingCell('right', 7, '4 · 12', 48, 8, '+8 pkt')
cells[0][8] = new ChallengeCell('right')
cells[0][9] = new BuildingCell('right', 7, '12 · 6', 72, 8, '+8 pkt')
cells[0][10] = new ChallengeCell('right')
cells[0][11] = new BuildingCell('right', 8, '8 · 8', 64, 9, '+9 pkt')
cells[0][12] = new BuildingCell('right', 8, '9 · 8', 72, 9, '+9 pkt')
cells[0][13] = toJailCell
cells[1][13] = new BuildingCell('down', 8, '9 · 9', 81, 9, '+9 pkt')
cells[2][13] = new BuildingCell('down', 9, '9 · 12', 108, 10, '+10 pkt')
cells[3][13] = new BuildingCell('down', 9, '11 · 12', 132, 10, '+10 pkt')
cells[4][13] = new BuildingCell('down', 9, '11 · 11', 121, 10, '+10 pkt')

export default {
  name: 'Game',
  props: ['players'],
  data() {
    return {
      dices: [6, 6],
      cells: cells,
      throws: 0,
      turn: 0,
      gamePlayers: [],
      challenge: null,
      isModalVisible: false,
      isEndModalVisible: false,
      endGame: false,
      move: false
    }
  },
  components: {
    Cell,
    Player,
    Dice,
    Modal,
    End
  },
  computed: {
    currentPlayer() {
      return this.gamePlayers[this.turn % this.players.length];
    }
  },
  mounted() {
    const playersTab = []
    for (const p of this.players) {
      playersTab.push(new DesktopPlayer(p.id, p.name));
    }
    this.gamePlayers = playersTab;
  },
  methods: {
    roll() {
      this.dices = [randomDice(), randomDice()];
      this.movePlayer();
    },
    movePlayer() {
      this.move = true
      let steps = this.dices.reduce((acc, val) => acc + val)
      let cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]
      this.challenge = null;
      const stepInterval = setInterval(() => {
        switch (cell.moveDir) {
          case 'up':
            this.currentPlayer.i -= 1
            break
          case 'down':
            this.currentPlayer.i += 1
            break
          case 'right':
            this.currentPlayer.j += 1
            break
          case 'left':
            this.currentPlayer.j -= 1
            break
          default:
            throw(`Unknown moveDir ${cell.moveDir}`)
        }
        cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]
        cell.onStepOverStart(this.currentPlayer)
        steps -= 1
        if (steps === 0) {
          clearInterval(stepInterval)
          if (!cell.description && cell.name !== 'challenge') {
            switch (cell.name) {
              case 'parking':
                this.challenge = null;
                break
              case 'jail':
                this.challenge = null;
                break
              case 'toJail':
                this.currentPlayer.j -= 13
                this.currentPlayer.points -= 30
                break
            }
          } else if (cell.name === 'challenge') {
            this.challenge = Challenges[randomInteger(0, 9)]
          }

          const [d1, d2] = this.dices
          if (d1 !== d2) {
            this.throws = 0;
            this.onStep(cell);
          } else {
            this.throws += 1
            if (this.throws !== 3) {
              this.onStep(cell);
            } else {
              this.throws = 0;
              this.currentPlayer.points -= 30
              this.currentPlayer.i = 0
              this.currentPlayer.j = 0
              this.turn += 1;
            }
          }
        }
      }, 10)
    },
    onStep(cell) {
      if (this.challenge) {
        const element = document.createElement('p');
        const text = document.createTextNode(this.challenge.description + '?');
        element.appendChild(text);
        this.$refs.inputWrapper.prepend(element);
      }
      if (cell.description || this.challenge) {
        setTimeout(() => this.$refs.buttonRole.style.pointerEvents = "none", 0);
        setTimeout(() => this.$refs.inputWrapper.style.display = "flex", 0);
      } else {
        this.throws !== 0 ? null : this.turn += 1;
      }
    },
    answer() {
      const cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]
      const input = document.getElementById('input');
      const oldPoints = this.currentPlayer.points;

      this.$refs.inputWrapper.style.display = "none";
      if (input.value == cell.correctAnswer || (this.challenge ? (this.challenge.correctAnswer.indexOf(input.value) != -1) : false)) {
        if (cell.points) {
          this.currentPlayer.points += cell.points;
        } else {
          this.currentPlayer.points += 10
        }
        setTimeout(() => this.$refs.text1.style.display = "flex", 0);
        setTimeout(() => this.$refs.text1.style.display = "none", 1000);
        this.challenge = null;
      } else {
        setTimeout(() => this.$refs.text2.style.display = "flex", 0);
        setTimeout(() => this.$refs.text2.style.display = "none", 1000);
      }

      const [d1, d2] = this.dices
      if (d1 !== d2 || (d1 === d2 && oldPoints === this.currentPlayer.points)) {
        this.throws = 0;
        this.turn += 1;
      }

      this.move = false
    },
    showModal() {
      this.isModalVisible = true
    },
    closeModal() {
      this.isModalVisible = false
    },
    ifEndGameOpen() {
      this.isEndModalVisible = true
    },
    ifEndGameClose() {
      this.isEndModalVisible = false
    },
    end() {
      this.endGame = true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.desk {
  width: 95%;
  height: 95vh;
  display: grid;
  grid-template-rows: repeat(6, 1fr);
}

.desk > tr {
  width: calc(100% - 1px);
  display: flex;
  align-items: stretch;
  justify-content: space-between;
}

.cell {
  border: 1px solid black;
  width: calc((100% / 14) - 2px);
  height: calc(100% - 2px);
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  align-content: flex-start;
  justify-content: flex-start;
}

.options-wrapper {
  width: calc(100% - 2*((100% / 14) - 2px));
  display: flex;
  justify-content: right;
}

.options {
  width: fit-content;
  height: fit-content;
  display: flex;
  justify-content:flex-end;
  align-items: flex-start;
  margin: 1rem 0;
}

.modal-ifEnd-wrapper {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2;
  cursor: pointer;
}

.modal-ifEnd {
  background: #FFFFFF;
  box-shadow: 2px 2px 20px 1px;
  display: flex;
  flex-direction: column;
  border-radius: 30px/10px;
  cursor: default;
  width: 50vw;
  height: 30vh;
  align-items: center;
}

.modal-ifEnd-header {
  border-bottom: 1px solid #eeeeee;
  color: #4AAE9B;
  padding: 1.5rem 2rem;
}

.modal-ifEnd-body {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  width: 50%;
}

.modal-ifEnd-button {
  height: fit-content;
  padding: 0.8rem 2.5rem;
  cursor: pointer;
}

.button-info,
.button-close {
  height: fit-content;
  width: fit-content;
  padding: 0.5rem;
  border-radius: 30px/10px;
  cursor: pointer;
}

.button-close {
  margin: 0 0.8rem;
}

.inner {
  width: 30%;
  display: flex;
  flex-direction: column;
  position: absolute;
  left: 0;
  right: 0;
  margin: auto;
}

.title {
  text-align: center;
  margin-bottom: 2rem;
  font-size: 3em;
}

@media (min-width: 1600px) {
  .title {
    margin-bottom: .5rem;
  }
}

.players-info {
  border-collapse: collapse;
  table-layout: fixed;
  width: 100%;
}

.players-info td {
  width: 100%;
}

.table-points {
  border: solid black 1px;
  text-align: right;
  padding: 0 5px;
}

.table-name {
  text-align: center;
}

.actions {
  display: flex;
  flex-direction: column;
  align-content: center;
}

.actions-role,
.actions-input {
  display: flex;
  width: 100%;
  justify-content: space-evenly;
  margin-top: 1rem;
}

.dice {
  font-size: 32px;
  margin-left: 0.5rem;
}

.actions-role button,
.actions-input button {
  width: 6rem;
  cursor: pointer;
}

.actions-input input {
  padding: 0.2rem 0.5rem;
}

.actions-input,
.actions-text1,
.actions-text2 {
  display: none;
}

.actions-text1,
.actions-text2 {
  align-self: center;
  margin-top: 1rem;
  font-weight: bold;
  font-size: 1.5em;
}

.actions-text1 {
  color: #00FF00;
}

.actions-text2 {
  color: #FF0000;
}

.turn {
  font-size: 1.5em;
}
</style>
