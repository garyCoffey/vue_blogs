<template>
  <v-container fluid>
    <ShowContainer
      index-btn-text="All Articles"
      index-btn-path="/blogs"
      :content="article"
      :prev="prev"
      :next="next"
    />
  </v-container>
</template>
<script>
export default {
  async asyncData ({ $content, params }) {
    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()
    const article = await $content('articles', params.slug).fetch()
    return {
      article,
      prev,
      next
    }
  },
  head () {
    return {
      title: this.article.title,
      meta: [
        { hid: 'description', name: 'description', content: this.article.description },
        // Open Graph
        { hid: 'og:title', property: 'og:title', content: this.article.title },
        { hid: 'og:description', property: 'og:description', content: this.article.description },
        // Twitter Card
        { hid: 'twitter:title', name: 'twitter:title', content: this.article.title },
        { hid: 'twitter:description', name: 'twitter:description', content: this.article.description }
      ]
    }
  }
}
</script>
<style>
</style>
