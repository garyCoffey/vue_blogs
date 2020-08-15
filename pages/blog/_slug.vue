<template>
  <article>
    <div>
      <img
        :src="article.img"
        :alt="article.alt"
      />
      <div>

      </div>
      <div>
        <NuxtLink
          to="/blogs"
        >
          All articles
        </NuxtLink>
      </div>
    </div>
    <div>
      <h1>{{ article.title }}</h1>
      <p>{{ article.description }}</p>
      <p>{{ formatDate(article.updatedAt) }}</p>
      <p v-if=false>Post last updated: {{ formatDate(article.updatedAt) }}</p>
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
    </div>
  </article>
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
