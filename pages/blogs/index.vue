<template>
  <v-container fluid>
    <v-sheet>
      <h2>Blog Posts</h2>
      <p>The blog posts you will find here relate to all things code. In the future I might expand into other topics but we'll see.</p>
      <IndexContainer :items="articles" />
    </v-sheet>
  </v-container>
</template>

<script>
export default {
  async asyncData ({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .only(
        [
          'title',
          'description',
          'img',
          'alt',
          'categories',
          'tags',
          'github_name',
          'createdAt',
          'type',
          'updatedAt',
          'slug'
        ])
      .sortBy('updatedAt', 'desc')
      .fetch()
    return {
      articles
    }
  }
}
</script>
<style>
</style>
