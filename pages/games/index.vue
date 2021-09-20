<template>
  <v-container fluid >
    <h2>Games</h2>
    <IndexContainer :items="gameConfigs" type='game'/>
  </v-container>
</template>
<script>
  const gameConfigs = [
    {
      title: 'Monster Slayer',
      description: 'A turn based game where you will fight to defeat the monster, before you yourself are defeated. Something to mention is that I followed a Vue tutorial for the main parts of this game. It has deviated slightly from that tutorial but not by much.',
      img: 'https://media.giphy.com/media/NXOVucluUUdeE/giphy.gif',
      alt: 'Frankenstein smoking',
      createdAt: 'September 19, 2021',
      to: '/games/monsterSlayer'
    },
    {
      title: 'TicTacToe',
      description: 'Race against your opponent (computer or friend) to get 3 in a row!',
      img: 'https://media.giphy.com/media/Yj97rLWiH29Hi/giphy.gif',
      alt: 'Tic Tac Toe on computer terminal',
      createdAt: 'September 19, 2021',
      to: '/games/ticTacToe'
    }
  ]
  export default {
    data() {
      return {
        show: false,
        gameConfigs
      }
    }
  }
</script>
