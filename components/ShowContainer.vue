<template>
  <v-container fluid>
    <v-card
      id="card"
      flat
      elevation="0"
    >
      <VImgUi
        :src="content.img ? content.img : '/defaultPic.png'"
        :alt="content.alt ? content.alt : 'VSCode screenshot'"
      />
      <!-- <TableOfContents :toc="content.toc" /> -->
      <VBtnUi
        id="all-items"
        name="all-items"
        :title="indexBtnText"
        nuxt
        link
        :to="indexBtnPath"
      />
      <v-card-title>{{ content.title }}</v-card-title>
      <v-card-subtitle>{{ formatDate(content.updatedAt) }}</v-card-subtitle>
      <v-card-subtitle v-if="false">
        Post last updated: {{ formatDate(content.updatedAt) }}
      </v-card-subtitle>
      <v-card-text>
        {{ content.description }}
      </v-card-text>
    </v-card>
    <v-container fluid>
      <nuxt-content :document="content" />
      <PrevNext
        v-if="content.type === 'blog'"
        :prev="prev"
        :next="next"
      />
    </v-container>
  </v-container>
</template>

<script>

export default {
  props: {
    indexBtnText: {
      type: String,
      required: true
    },
    indexBtnPath: {
      type: String,
      required: true
    },
    content: {
      type: Object,
      required: true
    },
    prev: {
      type: Object,
      required: false,
      default: () => {
        return {}
      }
    },
    next: {
      type: Object,
      required: false,
      default: () => {
        return {}
      }
    }
  },
  methods: {
    formatDate (date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }
}
</script>

<style scoped>
  @media only screen and (min-width: 600px) {
    #all-items {
      text-align: center;
      margin-left: auto;
      margin-right: auto;
      margin-top: 30px;
    }
    #card {
      padding: 10px;
      margin-left: auto;
      margin-right: auto;
      width: 8em;
      width: 700px;
      align-content: center;
    }
  }
</style>
