<template>
  <v-container fluid>
    <v-card
      flex
      d-flex
      flex-column
      hover
      nuxt
      link
      :to="to"
    >
      <v-img
        class="white--text align-end"
        height="200px"
        :src="img ? img : '/defaultPic.png'"
        :alt="alt ? alt : 'VSCode screenshot'"
      />
      <v-card-title
        class="text--primary flex"
      >{{title}}
      </v-card-title>

      <v-card-subtitle class="pb-0">{{formatDate(date)}}</v-card-subtitle>

      <v-card-text
        class="text--primary flex"
      >{{description}}
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    props: [
      'title',
      'description',
      'img',
      'alt',
      'date',
      'to'
    ],
    methods: {
      formatDate(date) {
        const options = { year: 'numeric', month: 'long', day: 'numeric' }
        return new Date(date).toLocaleDateString('en', options)
      }
    }
  }
</script>
<style scoped>
  .text--primary {
    overflow-y: auto;
    height:100px
  }
</style>