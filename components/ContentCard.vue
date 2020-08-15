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
    :src="img ? `/thumbnails/${img}`: '/defaultPic.png'"
    />
    <v-card-title>{{title}}</v-card-title>

    <v-card-subtitle class="pb-0">{{formatDate(date)}}</v-card-subtitle>

    <v-card-text class="text--primary">{{description}}</v-card-text>
  </v-card>
</template>

<script>
export default {
  props: ['img', 'title', 'date', 'description', 'to'],
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }
}
</script>