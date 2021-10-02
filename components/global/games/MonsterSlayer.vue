<template
  slot
  name="monster-slayer"
>
  <v-container id="monster-slayer">
    <v-alert
      dismissible
      :type="won ? 'success' : 'warning'"
      :value="won || monsterWon"
    >
      {{ won ? 'You Have Won the Game!!!' : 'The monster has won!!!' }}
    </v-alert>
    <v-container
      elevation="2"
    >
      <v-row>
        <div class="small-6 columns">
          <h2 class="text-center">
            You
          </h2>
          <div class="healthbar">
            <div class="healthbar health text-center" :style="{width: `${playerHealth}px` }">
              {{ playerHealth }}
            </div>
          </div>
        </div>
      </v-row>
      <v-row>
        <div class="small-6 columns">
          <h2 class="text-center">
            Monster
          </h2>
          <div class="healthbar">
            <div class="healthbar health text-center" :style="{width: `${monsterHealth}px` }">
              {{ monsterHealth }}
            </div>
          </div>
        </div>
      </v-row>
      <v-row class="controls">
        <div v-if="turn || checkGameOver" class="small-12 columns" @click="turn = !turn; coolOff -= 1">
          <v-btn
            id="attack"
            name="attack"
            @click="attack"
          >
            ATTACK
          </v-btn>
          <v-btn
            v-if="checkCoolOff"
            id="special-attack"
            name="special-attack"
            @click="specialAttack()"
          >
            SPECIAL ATTACK
          </v-btn>
          <v-btn
            id="heal"
            name="heal"
            @click="heal()"
          >
            HEAL
          </v-btn>
          <v-btn
            id="give-up"
            name="give-up"
            @click="giveUp()"
          >
            GIVE UP
          </v-btn>
        </div>
      </v-row>
      <v-row class="controls">
        <div class="small-12 columns">
          <v-btn
            id="start-game"
            name="start-game"
            @click="resetGame"
          >
            START NEW GAME
          </v-btn>
        </div>
      </v-row>
      <v-row
        v-for="log in logs"
        :key="log.id"
        class="log"
      >
        <div class="small-12 columns" :style="{backgroundColor: checkColor(log) ? 'red' : 'blue'}">
          {{ log }}
        </div>
      </v-row>
    </v-container>
  </v-container>
</template>

<script>
const constants = Object.freeze({
  PLAYER_HEALTH: 100,
  MONSTER_HEALTH: 100,
  MONSTER_MAX: 12,
  MONSTER_MIN: 7,
  MAX_ATTACK: 10,
  MIN_ATTACK: 5,
  MAX_SP_ATTACK: 15,
  MIN_SP_ATTACK: 10,
  HEAL: 10
})

export default {
  data () {
    return {
      playerHealth: constants.PLAYER_HEALTH,
      monsterHealth: constants.MONSTER_HEALTH,
      coolOff: 0,
      logs: [],
      won: false,
      monsterWon: false,
      turn: true,
      gameOver: false
    }
  },
  computed: {
    // eslint-disable-next-line vue/return-in-computed-property
    checkGame () {
      if (this.monsterHealth <= 0) {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.gameOver = true
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.won = true
        this.resetGame()
      } else if (this.playerHealth <= 0) {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.gameOver = true
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.monsterWon = true
        this.resetGame()
      } else {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.turn = true
      }
    },
    checkGameOver () {
      return this.gameOver
    },
    checkCoolOff () {
      return this.coolOff <= 0
    }
  },
  methods: {
    attack () {
      const randomNumber = Math.floor(Math.random() * (constants.MAX_ATTACK - constants.MIN_ATTACK + 1) + constants.MIN_ATTACK)
      this.monsterHealth -= randomNumber
      this.createLog('PLAYER', 'HITS', randomNumber)
      this.monsterTurn()
    },
    specialAttack () {
      const randomNumber = Math.floor(Math.random() * (constants.MAX_SP_ATTACK - constants.MIN_SP_ATTACK + 1) + constants.MIN_SP_ATTACK)
      this.monsterHealth -= randomNumber
      this.coolOff = 4
      this.createLog('PLAYER', 'SPECIAL HITS', randomNumber)
      this.monsterTurn()
    },
    heal () {
      if (this.playerHealth === 100) {
        return
      }
      this.playerHealth += 10
      this.createLog('PLAYER', 'HEALS', 10)
      this.monsterTurn()
    },
    giveUp () {
      this.playerHealth = 0
      setTimeout(() => {
        return this.checkGame
      }, 300)
    },
    monsterTurn () {
      setTimeout(() => {
        const randomNumber = Math.floor(Math.random() * (constants.MONSTER_MAX - constants.MONSTER_MIN + 1) + constants.MONSTER_MIN)
        this.playerHealth -= randomNumber
        this.createLog('MONSTER', 'HITS', randomNumber)
      }, 300)
      setTimeout(() => {
        return this.checkGame
      }, 300)
    },
    createLog (person, action, amount) {
      this.logs.unshift(`${person} ${action}, for ${amount}`)
    },
    checkColor (log) {
      return log.split(' ')[0] === 'MONSTER'
    },
    resetGame () {
      this.playerHealth = constants.PLAYER_HEALTH
      this.monsterHealth = constants.MONSTER_HEALTH
      this.coolOff = 0
      this.turn = true
      this.logs = []
      this.gameOver = false
    }
  }
}
</script>

<style>
  .text-center {
    text-align: center;
  }

  .healthbar {
    width: 100%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
  }

  .health {
    background-color: green !important;
    margin: 0;
    color: white;
  }

  .controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
  }

  .turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
  }

  .log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
  }

  .log ul li {
    margin: 5px;
  }

  .log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
  }

  .log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
  }

  v-btn {
    font-size: 20px;
    background-color: #eee;
    padding: 12px !important;
    box-shadow: 0 1px 1px black;
    margin: 10px;
  }

  #start-game {
    background-color: #aaffb0;
  }

  #start-game:hover {
    background-color: #76ff7e;
  }

  #attack {
    background-color: #ff7367;
  }

  #attack:hover {
    background-color: #ff3f43;
  }

  #special-attack {
    background-color: #ffaf4f;
  }

  #special-attack:hover {
    background-color: #ff9a2b;
  }

  #heal {
    background-color: #aaffb0;
  }

  #heal:hover {
    background-color: #76ff7e;
  }

  #give-up {
    background-color: #ffffff;
  }

  #give-up:hover {
    background-color: #c7c7c7;
  }
</style>
