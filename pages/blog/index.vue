<template>
  <article>
    <div>
      <img
        :src="article.img"
        :alt="article.alt"
      />

      </div>
      <div>
        <NuxtLink
          to="/blogs"
        >
          All articles
        </NuxtLink>
        <a
          href="https://nuxtjs.org/blog/creating-blog-with-nuxt-content"
        >
          Tutorial
        </a>
      </div>
    <div>
      <p>Post last updated: {{ formatDate(article.updatedAt) }}</p>
      <nav>
        <ul>
          <li
            v-for="link of article.toc"
            :key="link.id"
          >
            <nuxtLink
              :to="`#${link.id}`"
              >{{ link.text }}</nuxtLink
            >
          </li>
        </ul>
      </nav>
      <nuxt-content :document="article" />
      <PrevNext :prev="prev" :next="next"/>
    </div>
  </article>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()
    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()
    return {
      article,
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