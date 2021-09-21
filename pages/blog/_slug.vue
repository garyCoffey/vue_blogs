<template>
  <v-container>
    <v-card
      flat
    >
      <VImg
        :src="article.img"
        :alt="article.alt"
      >
      </VImg>
      <v-btn
        nuxt
        link
        to="/blogs"
      >
        All articles
      </v-btn>
      <v-card-title>{{ article.title }}</v-card-title>
      <v-card-subtitle>{{ formatDate(article.updatedAt) }}</v-card-subtitle>
      <v-card-subtitle v-if=false>Post last updated: {{ formatDate(article.updatedAt) }}</v-card-subtitle>
      <v-card-text>{{ article.description }}</v-card-text>
      <TableOfContent :toc="article.toc" />
      <nuxt-content :document="article" />
      <PrevNext :prev="prev" :next="next"/>
    </v-card>
  </v-container>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
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
  head() {
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
