<template>
  <v-card
  class="mx-auto"
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
    <v-card-title>{{title}}</v-card-title>

    <v-card-subtitle class="pb-0">{{formatDate(date)}}</v-card-subtitle>

    <v-card-text class="text--primary">{{description}}</v-card-text>
  </v-card>
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