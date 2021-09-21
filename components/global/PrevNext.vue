<template>
  <v-container>
    <v-row>
      <v-btn
        v-if="prev"
        nuxt
        link
        :to="{ name: 'blog-slug', params: { slug: prev.slug } }"
      >
        {{ prev.title }}
      </v-btn>
      <v-btn
        v-if="next"
        nuxt
        link
        :to="{ name: 'blog-slug', params: { slug: next.slug } }"
      >
        {{ next.title }}
      </v-btn>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
    prev: {
      type: Object,
      default: () => null
    },
    next: {
      type: Object,
      default: () => null
    }
  }
}
</script>