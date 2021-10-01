<template
  slot
  name="tic-tac-toe"
>
  <div id="tic-tac-toe">
    <v-alert
      v-model="alert"
      dismissible
      :type="draw() ? 'warning' : 'success'"
      :value="alert"
    >{{alertMessage}}
    </v-alert>
    <v-card
      elevation="2"
      :disabled="alert || gameResetNeeded"
    >
      <h2>Current Player is {{currentPlayer}}</h2>
      <br />
      <v-container fluid>
        <v-row>
          <v-btn
            id="game-button-1"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button1')"
            v-bind:style="{backgroundImage: backgrounds['button1']}"
          >
          </v-btn>
          <v-btn
            id="game-button-2"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button2')"
            v-bind:style="{backgroundImage: backgrounds['button2']}"
          >
          </v-btn>
          <v-btn
            id="game-button-3"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button3')"
            v-bind:style="{backgroundImage: backgrounds['button3']}"
          >
          </v-btn>
        </v-row>
        <v-row>
          <v-btn
            id="game-button-4"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button4')"
            v-bind:style="{backgroundImage: backgrounds['button4']}"
          >
          </v-btn>
          <v-btn
            id="game-button-5"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button5')"
            v-bind:style="{backgroundImage: backgrounds['button5']}"
          >
          </v-btn>
          <v-btn
            id="game-button-6"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button6')"
            v-bind:style="{backgroundImage: backgrounds['button6']}"
          >
          </v-btn>
        </v-row>
        <v-row>
          <v-btn
            id="game-button-7"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button7')"
            v-bind:style="{backgroundImage: backgrounds['button7']}"
          >
          </v-btn>
          <v-btn
            id="game-button-8"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button8')"
            v-bind:style="{backgroundImage: backgrounds['button8']}"
          >
          </v-btn>
          <v-btn
            id="game-button-9"
            class="game-button"
            height="100px"
            width="100px"
            @click="move('button9')"
            v-bind:style="{backgroundImage: backgrounds['button9']}"
          >
          </v-btn>
        </v-row>
        <v-row >
          <v-btn
            :disabled="gameInPlay()"
            @click="toggleGameMode()"
          > Current Game Mode is {{gameMode}}
          </v-btn>
      </v-row>
      </v-container>
      <br />

    </v-card>
    <br />
    <div class="small-6 columns">
      <v-btn
        @click="resetGame()"
      > Reset Game
      </v-btn>
    </div>
  </div>
</template>

<script>
  const playerX = 'X'
  const playerO = 'O'
  const notPlayed = '-'
  const playerXBackground = 'url(/content-images/games/ticTacToe/x.png)'
  const playerOBackground = 'url(/content-images/games/ticTacToe/o.png)'
  const notPlayedBackground = 'url(/content-images/games/ticTacToe/nobody.png)'
  const defaultGameMode = 'Computer'
  const friendGameMode = 'Friend'

  const constants = Object.freeze({
      BUTTON_BACKGROUNDS: {
        button1: notPlayedBackground,
        button2: notPlayedBackground,
        button3: notPlayedBackground,
        button4: notPlayedBackground,
        button5: notPlayedBackground,
        button6: notPlayedBackground,
        button7: notPlayedBackground,
        button8: notPlayedBackground,
        button9: notPlayedBackground
      },
      BUTTON_VALUES: {
        button1: notPlayed,
        button2: notPlayed,
        button3: notPlayed,
        button4: notPlayed,
        button5: notPlayed,
        button6: notPlayed,
        button7: notPlayed,
        button8: notPlayed,
        button9: notPlayed
      },
      PLAYER1: playerX,
      GAME_MODE: defaultGameMode,
      MOVES: 0,
      GAME_RESET_NEEDED: false
    });

  export default {
    data() {
      return {
        currentPlayer: constants.PLAYER1,
        backgrounds: constants.BUTTON_BACKGROUNDS,
        buttonValues: constants.BUTTON_VALUES,
        alertMessage: '',
        alert: false,
        gameMode: constants.GAME_MODE,
        moves: constants.MOVES,
        gameResetNeeded: constants.GAME_RESET_NEEDED
      }
    },
    methods: {
      move: function(button) {
        if ( this.buttonValues[button] !== notPlayed) return
        this.moves++;
        this.buttonValues[button] = this.currentPlayer;
        this.backgrounds[button] = this.getButtonBackground()
        if (this.checkForWinner() || this.draw()) return this.handleEndGame()
        this.currentPlayer = this.currentPlayer === playerX ? playerO : playerX
        if (this.gameMode === defaultGameMode && this.currentPlayer !== playerX) return this.move(this.computerMove())
      },
      getButtonBackground: function() {
        if (this.currentPlayer === playerX) return playerXBackground
        else if (this.currentPlayer === playerO) return playerOBackground
        else return notPlayedBackground
      },
      handleEndGame: function() {
        this.alertMessage = this.draw() ? `Game was a draw after ${this.moves} moves!!` : `${this.currentPlayer} has WON in ${this.moves} moves!!!`
        this.alert = !this.alert
        this.gameResetNeeded = true;
      },
      checkVerticalWinner: function() {
        return (
                 (
                   this.buttonValues['button1'] === this.currentPlayer &&
                   this.buttonValues['button4'] === this.currentPlayer &&
                   this.buttonValues['button7'] === this.currentPlayer
                 ) ||
                 (
                   this.buttonValues['button2'] === this.currentPlayer &&
                   this.buttonValues['button5'] === this.currentPlayer &&
                   this.buttonValues['button8'] === this.currentPlayer
                 ) ||
                 (
                   this.buttonValues['button3'] === this.currentPlayer &&
                   this.buttonValues['button6'] === this.currentPlayer &&
                   this.buttonValues['button9'] === this.currentPlayer
                 )
               )
      },
      checkHorizontalWinner: function() {
        return (
                 (
                   this.buttonValues['button1'] === this.currentPlayer &&
                   this.buttonValues['button2'] === this.currentPlayer &&
                   this.buttonValues['button3'] === this.currentPlayer
                 ) ||
                 (
                   this.buttonValues['button4'] === this.currentPlayer &&
                   this.buttonValues['button5'] === this.currentPlayer &&
                   this.buttonValues['button6'] === this.currentPlayer
                 ) ||
                 (
                   this.buttonValues['button7'] === this.currentPlayer &&
                   this.buttonValues['button8'] === this.currentPlayer &&
                   this.buttonValues['button9'] === this.currentPlayer
                 )
               )
      },
      checkDiagonalWinner: function() {
        return (
                 (
                   this.buttonValues['button1'] === this.currentPlayer &&
                   this.buttonValues['button5'] === this.currentPlayer &&
                   this.buttonValues['button9'] === this.currentPlayer
                 ) ||
                 (
                   this.buttonValues['button3'] === this.currentPlayer &&
                   this.buttonValues['button5'] === this.currentPlayer &&
                   this.buttonValues['button7'] === this.currentPlayer
                 )
               )
      },
      checkForWinner: function() {
        return this.checkDiagonalWinner() || this.checkHorizontalWinner() || this.checkVerticalWinner();
      },
      computerMove: function() {
        let keys = Object.keys(this.buttonValues);
        let move = keys[Math.floor(Math.random()*keys.length)]
        if (this.buttonValues[move] !== notPlayed) return this.computerMove()
        return move;
      },
      toggleGameMode: function() {
        if (this.gameMode === defaultGameMode) {
          this.gameMode = friendGameMode
        }
        else this.gameMode = defaultGameMode
      },
      resetGame: function() {
        Object.keys(constants.BUTTON_BACKGROUNDS).map(function(key, _) {
          constants.BUTTON_BACKGROUNDS[key] = notPlayedBackground;
        });
        Object.keys(constants.BUTTON_VALUES).map(function(key, _) {
          constants.BUTTON_VALUES[key] = notPlayed;
        });
        this.currentPlayer = constants.PLAYER1;
        this.moves = 0;
        this.gameResetNeeded = false;
        this.alert = false;
        this.alertMessage = '';
      },
      draw: function() {
        return  this.moves === 9 && !this.checkForWinner()
      },
      gameInPlay: function() {
        return this.moves > 0 &&
        !this.checkForWinner() &&
        !this.draw()
      }
    }
  }
</script>

<style>
  #tic-tac-toe {
    width: 350px;
  }
  .row {
    width: 100%;
  }
  button #game-button-1 .game-button v-btn {
    background-size: 150px;
  }
</style>
