<template
  slot
  name="v-img-ui"
>
  <v-img
    id="image-ui"
    class="white--text align-end"
    height="200px"
    width="410px"
    :src="src"
    :alt="alt"
  />
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      required: true
    },
    alt: {
      type: String,
      required: true
    }
  }
}
</script>
