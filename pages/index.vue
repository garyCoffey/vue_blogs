<template>
  <div>
    <h1>Home</h1>
    <v-card
      elevation="2"
    >
      <v-card-title>
        Welcome to Play & Code
      </v-card-title>
      <v-card-subtitle>
        I hope you enjoy reading the blog posts and playing the available games!
      </v-card-subtitle>
    </v-card>
  </div>
</template>
<script>
  export default {
    head: {
    title: 'Home page',
    meta:
      [
        {
          hid: 'description',
          name: 'description',
          content: 'Explains what the site has to offer'
        }
      ],
    },
  }
</script>
