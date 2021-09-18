<template>
  <div>
    <h1>Blog Posts</h1>
    <v-card>
      <p>
        The blog posts you will find here relate to all things code. In the future I might expand into other topics but we'll see.
      </p>
    </v-card>
    <v-container>
        <v-row>
          <v-col
            v-for="article of articles"
            :key="article.slug"
            cols="6"
          >
            <ContentCard
              v-bind:img="article.img"
              v-bind:title="article.title"
              v-bind:description="article.description"
              v-bind:date="article.updatedAt > article.createdAt ? article.updatedAt : article.createdAt"
              v-bind:to="{ name: 'blog-slug', params: { slug: article.slug } }"
            />
          </v-col>
        </v-row>
    </v-container>
  </div>
</template>

<script>
  export default {
    async asyncData({ $content, params }) {
      const articles = await $content('articles', params.slug)
        .only(
          [
            'title',
            'description',
            'img',
            'alt',
            'categories',
            'github_name',
            'createdAt',
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