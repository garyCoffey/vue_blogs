<template>
  <v-btn
    nuxt
    link
    :to="to"
    color="secondary"
    x-large
  />
</template>

<script>
import { defineComponent } from "@vue/composition-api"

  export default defineComponent({
    props: ['to']
  })
</script>