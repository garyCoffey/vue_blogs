<template>
  <v-container>
    <div>
      <img
        :src="game.img"
        :alt="game.alt"
      />
      <div>
        <NuxtLink
          to="/games"
        >
          All Games
        </NuxtLink>
      </div>
    </div>
    <div>
      <h2>{{ game.title }}</h2>
      <p>{{ game.description }}</p>
      <p>{{ formatDate(game.updatedAt) }}</p>
      <p v-if=false>Post last updated: {{ formatDate(game.updatedAt) }}</p>
      <!-- <nav>
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
      </nav> -->
      <nuxt-content :document="game" />
    </div>
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
