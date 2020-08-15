<template>
    <div id="app">
      <section class="row">
          <div class="small-6 columns">
              <h1 class="text-center">YOU</h1>
              <div class="healthbar">
                  <div class="healthbar health text-center">
                    {{playerHealth}}
                  </div>
              </div>
          </div>
          <div class="small-6 columns">
              <h1 class="text-center">MONSTER</h1>
              <div class="healthbar">
                  <div class="healthbar health text-center">
                    {{ monsterHealth }}
                  </div>
              </div>
          </div>
      </section>
      <section class="row controls">
          <div class="small-12 columns" v-if="turn || checkGameOver" @click='turn = !turn; coolOff -= 1'>
              <button id="attack" @click="attack">ATTACK</button>
              <button id="special-attack" @click="specialAttack()" v-if="checkCoolOff">SPECIAL ATTACK</button>
              <button id="heal" @click="heal()">HEAL</button>
              <button id="give-up" @click="giveUp()">GIVE UP</button>
          </div>
      </section>
      <section class="row controls">
          <div class="small-12 columns">
              <button id="start-game" @click="resetGame">START NEW GAME</button>
          </div>
      </section>
      <section class="row log" v-for="log in logs">
          <div class="small-12 columns" :style="{backgroundColor: checkColor(log) ? 'red' : 'blue'}">
            {{log}}
          </div>
      </section>
  </div>
</template>

<script>
  import Vue from 'vue';

  const constants = Object.freeze({
   PLAYER_HEALTH: 100,
   MONSTER_HEALTH: 100,
   MONSTER_MAX: 12,
   MONSTER_MIN: 7,
   MAX_ATTACK: 10,
   MIN_ATTACK: 5,
   MAX_SP_ATTACK: 15,
   MIN_SP_ATTACK: 10,
   HEAL: 10,
});
new Vue({
  el: '',
  data: {
    playerHealth: constants.PLAYER_HEALTH,
    monsterHealth: constants.MONSTER_HEALTH,
    coolOff: 0,
    logs: [],
    turn: true,
    gameOver: false
  },
  methods: {
    attack: function() {
      console.log(Math.floor(Math.random()));
      randomNumber = Math.floor(Math.random() * (constants.MAX_ATTACK - constants.MIN_ATTACK + 1) + constants.MIN_ATTACK)
      this.monsterHealth -= randomNumber;
      this.createLog('PLAYER', 'HITS', randomNumber);
      this.monsterTurn();
    },
    specialAttack: function() {
      randomNumber = Math.floor(Math.random() * (constants.MAX_SP_ATTACK - constants.MIN_SP_ATTACK + 1) + constants.MIN_SP_ATTACK)
      this.monsterHealth -= randomNumber;
      this.coolOff = 4;
      this.createLog('PLAYER', 'SPECIAL HITS', randomNumber);
      this.monsterTurn();
    },
    heal: function() {
      if (this.playerHealth == 100) {
        return;
      }
      this.playerHealth += 10;
      this.createLog('PLAYER', 'HEALS', 10);
      this.monsterTurn();
    },
    giveUp: function() {
      this.playerHealth = 0;
      setTimeout(() => {
        this.checkGame;
      }, 600);
    },
    monsterTurn: function() {
      setTimeout(() => {
        randomNumber = Math.floor(Math.random() * (constants.MONSTER_MAX - constants.MONSTER_MIN + 1) + constants.MONSTER_MIN)
        this.playerHealth -= randomNumber;
        this.createLog('MONSTER', 'HITS', randomNumber);
      }, 600);
      setTimeout(() => {
        this.checkGame;
      }, 600);
    },
    createLog: function(person, action, amount) {
      this.logs.unshift(`${person} ${action}, for ${amount}`);
    },
    checkColor: function(log) {
      return log.split(' ')[0] == 'MONSTER';
    },
    resetGame: function() {
      this.playerHealth = constants.PLAYER_HEALTH;
      this.monsterHealth = constants.MONSTER_HEALTH;
      this.coolOff = 0;
      this.turn = true;
      this.logs = [];
      this.gameOver = false;
    }
  },
  computed: {
    checkGame: function() {
      if (this.monsterHealth <= 0) {
        this.gameOver = true;
        alert("You have WON!!!!")
        this.resetGame();
      } else if (this.playerHealth <= 0) {
        this.gameOver = true;
        alert("THE MONSTER HAS WON!!!!")
        this.resetGame();
      } else {
        this.turn = true;
      }
    },
    checkGameOver: function() {
      return this.gameOver;
    },
    checkCoolOff: function() {
      return this.coolOff <= 0;
    },
  }
});
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

button {
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

