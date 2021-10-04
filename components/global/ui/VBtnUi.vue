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
  >
    {{ title }}
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
    title: {
      type: String,
      required: true
    },
    className: {
      type: String,
      required: false,
      default: ''
    },
    type: {
      type: String,
      required: false,
      default: ''
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
      // eslint-disable-next-line vue/require-prop-type-constructor
      type: Object | String,
      required: false,
      default () {
        return ''
      }
    }
  }
}
</script>
