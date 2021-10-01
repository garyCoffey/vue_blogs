<template
  slot
  name="v-btn-ui"
>
  <v-btn
    :id="id"
    :class="className"
    :type="type"
    color="secondary"
    :name="name"
    :disabled="disabled"
    :nuxt="nuxt"
    :link="link"
    :to="to"
  >{{ buttonTitle }}
  </v-btn>
</template>

<script>
export default {
  props: {
    id: {
      type: String,
      required: true
    },
    name: {
      type: String,
      required: true
    },
    buttonTitle: {
      type: String,
      required: true
    },
    className: {
      type: String,
      required: false
    },
    type: {
      type: String,
      required: false
    },
    disabled: {
      type: Boolean,
      required: false
    },
    nuxt: {
      type: Boolean,
      required: false
    },
    link: {
      type: Boolean,
      required: false
    },
    to: {
      type: Object | String,
      required: false
    }
  }
}
</script>