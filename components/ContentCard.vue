<template>
  <v-container fluid>
    <v-card
      flex
      d-flex
      flex-column
      hover
      rounded
      nuxt
      link
      :to="to"
    >
      <VImgUi
        :src="img"
        :alt="alt"
      />
      <v-card-title
        class="text--primary flex"
      >{{title}}
      </v-card-title>

      <v-card-subtitle class="pb-0">{{formatDate(date)}}</v-card-subtitle>

      <v-card-text
        class="text--primary flex"
      >{{description}}
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    props: {
      title: {
        type: String,
        required: true
      },
      description: {
        type: String,
        required: true
      },
      img: {
        type: String,
        required: true
      },
      alt: {
        type: String,
        required: true
      },
      date: {
        type: String,
        required: true
      },
      to: {
        type: Object,
        required: true
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
<style scoped>
  .text--primary {
    overflow-y: auto;
    height:100px;
  }
</style>