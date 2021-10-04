---
title: VUE Coding tic-tac-toe
description: Coding out the humble tic tac toe game in VUE
img: /thumbnails/tic-tac-toe.webp
alt: tic-tac-toe
type: blog
tags: [blogs, code, vue]
categories: [blogs, code, vue]
---

### TicTacToe

The game that once saved the world from global thermonuclear war.... At least in the movies....

<v-img-ui src="https://media.giphy.com/media/RN6sYUh5VIYlG/giphy.gif" alt="war games">
  <template #v-img-ui>
  </template>
</v-img-ui>

Tic Tac Toe has been around for hundreds of years. Most people start playing this game as a kid due to how simple the rules are to understand. I chose to code this game for basically that same reason. I thought it would be super simple to create. Though it did not take me too long to complete, there was definitely more to it than I originally gave it credit for.

### Coding the Game

I coded the game by breaking it down into smaller chunks of work. Here is kind of the way it went for me:

- Create a game interactive game board/data pieces that would be needed (players)
- Make a move method that can be used on a click to run game logic
- Create a way to reset the game state
- Setup logic to check for a winner (basic at first)
- Create different game modes

That is the basic pattern I followed (from what I can remember at least)

So lets walk through some of these pieces of code.

#### Initial Setup

Ok so lets start off with the code.

```vue
<template>
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
          </v-btn>....
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
</script>
```

Something to note here is that this is the state of code as I am writing this. It definitely started of very basic and went through some revisions. So the first thing was to create some data objects for the game. I started with a nested array, but had trouble in the template with altering the look as the game was played... I found that an Object was a little easier to trigger the interface, though a nested array might have been the better data structure.

Also when I started out, I didn't put as much effort into styling. At first, I set color backgrounds (red/blue) to show the player moves. After I started getting the game logic all working, I later shifted to sprucing up the game look.

So basically what I have at this step is a UI game board, and some data setup to be manipulated in later steps.

### Game Logic

<v-img-ui src="https://media.giphy.com/media/10pOjRQeiyb0ZO/giphy.gif" alt="irobot">
  <template #v-img-ui>
  </template>
</v-img-ui>

Okay so this is the part where the code gets a little more interesting.

```js
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
      }
}
```

First, just picking apart the move function, remember this is triggered when a cell of the gameboard is clicked. We pass in the cell for the logic to run. First thing to do is to check if the cell has already been played... if so return (nice try!). Then we increase the total moves, just to keep track and show at the end. Then we set the cell value on the data object board to the player that played it, as well as updating the data object viewing board just so the UI will be useful (it would suck to have to try and remember who played where... though that would be a more difficult/interesting game... Ghost tic tac toe...).

We then check to see if there is a winner or no more moves to be played. If so we handle that, if not continue. Then we swap players to give the other person a turn. We also do a check to see if there are no more moves that can be made `draw()`. In the case of a draw, we also need to handle the end of the game. The final line relates to game mode, so we'll tackle that later.

The next method is just to extract some simple logic to keep the moves function from growing too much (and to limit some of its responsibility). The `getButtonBackground` function just gets the data stored in a variable for a players background to change (the red/blue or x/o).

The `handleEndGame` function just gives an indication that someone has won, and disables the board until it is reset by the player(s).

### Resetting Game State

I had to tackle this code at this point in order to code/test the game. Before this, it would take a page reload to get everything zeroed out, and that was getting annoying.

```js
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
  }
}
```

In this function, I reset the game board to the default values (background and played values), I reset the player to X, moves to 0, gameReset is no longer needed (we are doing that currently...), and finally kill the last games alert message of the winner.

### Winners Circle

<v-img-ui src="https://media.giphy.com/media/x0kMYoT7J31i8/giphy.gif" alt="Charlie Sheen winning">
  <template #v-img-ui>
  </template>
</v-img-ui>

```js
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
```

Okay so this seems like a lot of stuff, but it is pretty simple. First, we have the main function `checkForWinner`. It uses the other more private type functions to look at the different ways of winning. So if someone has won diagonally, horizontally, or vertically it would return true. Each of those methods take responsibility for there section to manually look at the game board data to see if 1 player has 3 spots in a row. Not too bad, right?

One thing to mention is at first I only implemented the diagonal winner check. THis was because I just wanted to get the method to be able to return a true or false, and circle back later. I like coding this way so if it has some sort of fundamental problem, I didn't waste time getting it fully fleshed out. Not as much emotional toll throwing stuff away then lol...

### Multiple Game Modes

The last step I did was the setup to allow players to play with a computer or friend. Going back to the <a href=#game-logic>Game Logic</a> part of this is to check in the move function if the mode is computer and, if so, call the move function a second time with the computers move. Recall that all the move function takes in is a button value that corresponds to a game cell. This made the computer mode easy to make (at least in easy mode).

```js
computerMove: function() {
  let keys = Object.keys(this.buttonValues);
  let move = keys[Math.floor(Math.random()*keys.length)]
  if (this.buttonValues[move] !== notPlayed) return this.computerMove()
  return move;
},
```

This is what I was kind of talking about as the easy mode. The computer plays pretty badly right now. It just looks for a random non-played cell and makes that move. It does this by selecting a random cell and if it picks an already played one, recursively calling itself to try again. This is also why it is important to have the `draw()` check.

### Conclusion

That is my round 1 implementation of tic tac toe. I think there are some future improvements that I might make. The biggest being adding additional game modes for a more difficult computer. Maybe the computer can check if it needs to block and do that. Otherwise, it can look for a move that would make more sense (like the cell next to a previously played move). Also, it could maybe try and play center cell if it is open.

But for v1, I think it is pretty cool. I will most likely move on to other game ideas before revisiting this 1. Here is a link to the [full code in Github though](https://github.com/garyCoffey/vue_blogs/blob/main/components/global/games/ticTacToe.vue#L1) if you would like to make your own modifications!
