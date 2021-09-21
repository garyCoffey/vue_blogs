<template>
  <v-container fluid>
    <h2>Games</h2>
    <v-card>
      <v-card-text>
        The blog posts you will find here relate to all things code. In the future I might expand into other topics but we'll see.
      </v-card-text>
    </v-card>
    <IndexContainer :items="games" />
  </v-container>
</template>

<script>
  export default {
    async asyncData({ $content, params }) {
      const games = await $content('games', params.slug)
        .only(
          [
            'title',
            'description',
            'img',
            'alt',
            'type',
            'categories',
            'github_name',
            'createdAt',
            'updatedAt',
            'slug'
          ])
        .sortBy('updatedAt', 'desc')
        .fetch()
      return {
        games
      }
    }
  }
</script>
<style>
</style>