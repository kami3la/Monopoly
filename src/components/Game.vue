<template>
  <table class="desk">
    <tr v-for='(row, i) in cells' :key='[row, i]'>
      <template v-for='(cell, j) in row' :key='[cell, j]'>
        <Cell class="cell"
          v-if='cell'
          :i='i'
          :j='j'
          :players='players'
          :description='cell.description'
          :id='cell.id'
          :points='cell.points'
          :name='cell.name'
          :image='cell.image'
          :correctAnswer='cell.correctAnswer'
        />
        <td class="inner" v-else-if='i==3 && j == 1'>
          <table class="players-info">
            <tr>
              <th v-for='player in players' :key='player.id'>
                <span v-show='player === currentPlayer' class="turn">↓</span>
              </th>
            </tr>
            <tr>
              <th v-for='player in players' :key='player.id'>
                <Player :id='player.id' />
              </th>
            </tr>
            <tr>
              <td v-for='player in players' :key='player.id'>{{ player.points }}</td>
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
              <input placeholder="Wpisz odpowiedź..." id="input" />
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
  constructor(id) {
    this.i = 5
    this.j = 13
    this.points = 0
    this.id = id
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
  data() {
    return {
      dices: [6, 6],
      cells: cells,
      throws: 0,
      turn: 0,
      players: [new DesktopPlayer(1), new DesktopPlayer(2), new DesktopPlayer(3), new DesktopPlayer(4)],
      challenge: ''
    }
  },
  components: {
    Cell,
    Player,
    Dice
  },
  computed: {
    currentPlayer() {
      return this.players[this.turn % this.players.length];
    }
  },
  methods: {
    roll() {
      this.dices = [randomDice(), randomDice()];
      this.movePlayer();
    },
    movePlayer() {
      if (this.throws === 2) {
        this.throws = 0;
        this.currentPlayer.points -= 30
        this.currentPlayer.i = 0
        this.currentPlayer.j = 0
        this.turn += 1;
      }
      let steps = this.dices.reduce((acc, val) => acc + val)
      let cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]
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
                break
              case 'jail':
                break
              case 'toJail':
                this.currentPlayer.j -= 13
                this.currentPlayer.points -= 30
                break
            }
            const [d1, d2] = this.dices
            if (d1 !== d2) {
              this.turn += 1;
            } else {
              this.throws += 1
            }
          } else if (cell.name === 'challenge') {
            this.challenge = Challenges[randomInteger(0, 1)]
            console.log(this.challenge.correctAnswer)
          }
          this.onStep(cell);
        }
      }, 10)
    },
    onStep(cell) {
      if (cell.description || this.challenge.description) {
        setTimeout(() => this.$refs.buttonRole.style.pointerEvents = "none", 0);
        setTimeout(() => this.$refs.inputWrapper.style.display = "flex", 0);
      }
    },
    answer() {
      const cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]
      const input = document.getElementById('input');
      const oldPoints = this.currentPlayer.points;

      this.$refs.inputWrapper.style.display = "none";
      if (input.value == cell.correctAnswer || (this.challenge.correctAnswer.indexOf(input.value) != -1)) {
        if (cell.points) {
          this.currentPlayer.points += cell.points;
        } else {
          this.currentPlayer.points += 10
        }
        setTimeout(() => this.$refs.text1.style.display = "flex", 0);
        setTimeout(() => this.$refs.text1.style.display = "none", 1000);
      } else {
        setTimeout(() => this.$refs.text2.style.display = "flex", 0);
        setTimeout(() => this.$refs.text2.style.display = "none", 1000);
      }
      const [d1, d2] = this.dices
      if (d1 !== d2 || (d1 === d2 && oldPoints === this.currentPlayer.points)) {
        this.turn += 1;
        this.throws = 0;
      } else {
        this.throws += 1
      }
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

.inner {
  width: 30%;
  display: flex;
  flex-direction: column;
  position: absolute;
  left: 0;
  right: 0;
  margin: auto;
}

.players-info {
  border-collapse: collapse;
  width: 100%;
}

.players-info td {
  border: solid black 1px;
  text-align: right;
  padding: 0 5px;
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
