<template>
  <v-container>
    <ShowContainer
      indexBtnText="All Games"
      indexBtnPath="/games"
      :content="game"
    />
  </v-container>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const [prev, next] = await $content('games')
      .only(['title', 'slug', 'categories'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()
    const game = await $content('games', params.slug).fetch()
    return {
      game,
      prev,
      next
    }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }
}
</script>
<style>
</style>
