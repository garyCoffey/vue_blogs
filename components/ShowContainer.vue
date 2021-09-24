<template>
  <v-sheet
  >
    <v-card
      flat
    >
      <VImg
        :src="content.img"
        :alt="content.alt"
      >
      </VImg>
      <VBtn
        :to="indexBtnPath"
      >{{indexBtnText}}
      </VBtn>
      <v-card-title>{{ content.title }}</v-card-title>
      <v-card-subtitle>{{ formatDate(content.updatedAt) }}</v-card-subtitle>
      <v-card-subtitle v-if=false>Post last updated: {{ formatDate(content.updatedAt) }}</v-card-subtitle>
      <v-card-text
        color="secondary"
      >{{ content.description }}</v-card-text>
      <TableOfContent :toc="content.toc" />
      <nuxt-content :document="content" />
      <PrevNext :prev="prev" :next="next"/>
    </v-card>
  </v-sheet>
</template>

<script>
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
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
      required: false
    },
    next: {
      type: Object,
      required: false
    }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    }
  }
})
</script>
