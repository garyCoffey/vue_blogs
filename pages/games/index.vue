<template>
  <v-container fluid>
    <v-sheet>
      <h2>Games</h2>
      <p>The blog posts you will find here relate to all things code. In the future I might expand into other topics but we'll see.</p>
      <IndexContainer :items="games" />
    </v-sheet>
  </v-container>
</template>

<script>
  export default {
    head: {
    title: 'Games page',
    meta:
      [
        {
          hid: 'description',
          name: 'description',
          content: 'Showcasing all of the games available!'
        }
      ],
    },
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