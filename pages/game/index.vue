<template>
  <v-container>
    <div>
      <img
        :src="game.image"
        :alt="game.alt"
      />

      </div>
      <div>
        <NuxtLink
          to="/blogs"
        >
          All games
        </NuxtLink>
      </div>
    <div>
      <p>Post last updated: {{ formatDate(game.updatedAt) }}</p>
      <nav>
        <ul>
          <li
            v-for="link of game.toc"
            :key="link.id"
          >
            <nuxtLink
              :to="`#${link.id}`"
              >{{ link.text }}</nuxtLink
            >
          </li>
        </ul>
      </nav>
      <nuxt-content :document="game" />
      <PrevNext :prev="prev" :next="next"/>
    </div>
  </v-container>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const game = await $content('games', params.slug).fetch()
    const [prev, next] = await $content('games')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()
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