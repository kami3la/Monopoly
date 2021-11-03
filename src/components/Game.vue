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
        />
        <td class="inner" v-else-if='i==3 && j == 1'>
          <table class="players-info">
            <tr>
              <th v-for='player in players' :key='player.id'>
                <span v-show='player === currentPlayer'>↓</span>
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
            <div class="dices">
              <Dice class="dice" v-for='(dice, i) in dices' :value='dice' :key="i" />
            </div>
            <button @click="roll">Rzuć kośćmi</button>
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

const randomInteger = function (min, max) {
  return Math.floor(min + Math.random() * (max + 1 - min))
}
const randomDice = () => randomInteger(1, 5)

class DesktopCell {
  constructor(moveDir, image) {
    this.moveDir = moveDir
    this.image = image
  }

  onStepOverStart() {
    // Does nothing by default
  }

  onStop() {
    // Does nothing by default
  }
}

class BuildingCell extends DesktopCell {
  constructor(moveDir, id, description, points, name) {
    super(moveDir)
    this.id = id
    this.description = description
    this.points = points
    this.name = name
  }

  onStop(player) {
    player.points += this.points
  }
}

class ChallengeCell extends DesktopCell {
  constructor(moveDir) {
    super(moveDir, 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867702/wykrzyknik_iapdmc.png')
  }
}

class ChanceCell extends DesktopCell {
  constructor(moveDir) {
    super(moveDir, 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867702/wykrzyknik_iapdmc.png')
  }
}

class StartCell extends DesktopCell {
  constructor(moveDir) {
    super(moveDir, 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867923/start_drivuq.png');
  }

  onStepOverStart(player) {
    return player.points += 150;
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

const parkingCell = new DesktopCell('up', '')
const jailCell = new DesktopCell('right', 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867760/wiezienie_wtpaoa.png')
const toJailCell = new DesktopCell('down', 'https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635867761/dowiezenia_bwkp0w.png')
const startCell = new StartCell('left', '')

let cells = new Array(6);

for (let i = 0; i < 6; i++) {
  cells[i] = new Array(14);
}

cells[5][13] = startCell
cells[5][12] = new BuildingCell('left', 1, '2 · 3', 2, '+2 pkt')
cells[5][11] = new ChallengeCell('left')
cells[5][10] = new BuildingCell('left', 1, '3 · 3', 2, '+2 pkt')
cells[5][9] = new BuildingCell('left', 2, '4 · 3', 3, '+3 pkt')
cells[5][8] = new ChanceCell('left')
cells[5][7] = new BuildingCell('left', 2, '4 · 4', 3, '+3 pkt')
cells[5][6] = new BuildingCell('left', 2, '5 · 4', 3, '+3 pkt')
cells[5][5] = new ChallengeCell('left')
cells[5][4] = new BuildingCell('left', 3, '5 · 5', 4, '+4 pkt')
cells[5][3] = new BuildingCell('left', 3, '6 · 4', 4, '+4 pkt')
cells[5][2] = new BuildingCell('left', 3, '3 · 7', 4, '+4 pkt')
cells[5][1] = new BuildingCell('left', 4, '7 · 6', 5, '+5 pkt')
cells[5][0] = parkingCell
cells[4][0] = new BuildingCell('up', 4, '5 · 7', 5, '+5 pkt')
cells[3][0] = new BuildingCell('up', 4, '4 · 9', 5, '+5 pkt')
cells[2][0] = new ChanceCell('up')
cells[1][0] = new BuildingCell('up', 5, '5 · 9', 6, '+6 pkt')
cells[0][0] = jailCell
cells[0][1] = new BuildingCell('right', 5, '6 · 8', 6, '+6 pkt')
cells[0][2] = new BuildingCell('right', 5, '7 · 7', 6, '+6 pkt')
cells[0][3] = new BuildingCell('right', 6, '8 · 7', 7, '+7 pkt')
cells[0][4] = new BuildingCell('right', 6, '5 · 11', 7, '+7 pkt')
cells[0][5] = new BuildingCell('right', 6, '6 · 9', 7, '+7 pkt')
cells[0][6] = new BuildingCell('right', 7, '7 · 9', 8, '+8 pkt')
cells[0][7] = new BuildingCell('right', 7, '4 · 12', 8, '+8 pkt')
cells[0][8] = new ChallengeCell('right')
cells[0][9] = new BuildingCell('right', 7, '12 · 6', 8, '+8 pkt')
cells[0][10] = new ChanceCell('right')
cells[0][11] = new BuildingCell('right', 8, '8 · 8', 9, '+9 pkt')
cells[0][12] = new BuildingCell('right', 8, '9 · 8', 9, '+9 pkt')
cells[0][13] = toJailCell
cells[1][13] = new BuildingCell('down', 8, '9 · 9', 9, '+9 pkt')
cells[2][13] = new BuildingCell('down', 9, '9 · 12', 10, '+10 pkt')
cells[3][13] = new BuildingCell('down', 9, '11 · 12', 10, '+10 pkt')
cells[4][13] = new BuildingCell('down', 9, '11 · 11', 10, '+10 pkt')

export default {
  name: 'Game',
  data() {
    return {
      dices: [6, 6],
      cells: cells,
      turn: 0,
      players: [new DesktopPlayer(1), new DesktopPlayer(2), new DesktopPlayer(3), new DesktopPlayer(4)]
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
      let steps = this.dices.reduce((acc, val) => acc + val)
      let cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]
      const stepIterval = setInterval(() => {
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
        if (steps <= 0) {
          clearInterval(stepIterval)
          cell.onStop(this.currentPlayer)
          const [d1, d2] = this.dices
          if (d1 !== d2) {
            // TODO: If dices are same for the third time go to jail
            this.turn += 1
          }
        }
      }, 10)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.desk {
  width: 90%;
  height: 90vh;
  display: grid;
  grid-template-rows: repeat(6, 1fr);
}

.desk > tr {
  width: 100%;
  display: flex;
  align-items: stretch;
  justify-content: space-between;
}

.desk .cell {
  border: 1px solid black;
  width: calc((100% - 28px) / 14);
  height: calc(100% - 2px);
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  align-content: flex-start;
}

.inner {
  width: 30%;
  display: flex;
  flex-direction: column;
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
  width: 100%;
  justify-content: space-evenly;
  margin-top: 1rem;
}

.dice {
  font-size: 32px;
  margin-left: 0.5rem;
}

.actions button {
  width: 6rem;
}
</style>
