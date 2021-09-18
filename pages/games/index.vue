<template>
  <div>
    <h1>Games</h1>
    <v-card
      class="mx-auto"
      max-width="344"
    >
      <v-img
        src="https://media.giphy.com/media/NXOVucluUUdeE/giphy.gif"
        height="200px"
      ></v-img>

      <v-card-title>
        Monster Slayer
      </v-card-title>

      <v-card-actions>
        <nav>
          <NuxtLink to="/games/monsterSlayer">
            <v-btn
              color="orange lighten-2"
              text
            >
              Play
            </v-btn>
          </NuxtLink>
        </nav>

        <v-spacer></v-spacer>

        <v-btn
          icon
          @click="show = !show"
        >
          <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>

          <v-card-text>
            A turn based game where you will fight to defeat the monster, before you yourself are defeated.
          </v-card-text>
        </div>
      </v-expand-transition>
    </v-card>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        show: false,
      }
    }
  }
</script>
