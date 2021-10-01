<template>
  <v-container
    id="prev-next"
    fluid
  >
    <v-row>
      <VBtnUi
        v-if="prev"
        id="previous"
        name="previous"
        :buttonTitle="prev.title"
        nuxt
        link
        :to="{ name: 'blog-slug', params: { slug: prev.slug } }"
      />
      <VBtnUi
        v-if="next"
        id="next"
        name="next"
        :buttonTitle="next.title"
        nuxt
        link
        :to="{ name: 'blog-slug', params: { slug: next.slug } }"
      />
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

<style scoped>
  @media only screen and (min-width: 600px) {
    #prev-next {
      padding: 10px;

      margin-left: auto;
      margin-right: auto;
      width: 8em;
      width: 700px;
      align-content: center;
    }
    #previous {
      margin: 10px;
    }
    #next {
      margin: 10px;
    }
  }
</style>