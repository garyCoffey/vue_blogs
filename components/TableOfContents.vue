<template>
  <v-container
    v-if="toc.length > 3"
    fluid
  >
    <v-tabs
      v-for="link of toc"
      :key="link.id"
      background-color="primary"
      right
      :to="`#${link.id}`"
    >
      <v-tabs-slider color="secondary" />
      <a
        role="tab"
        :href="`#${link.id}`"
      >
        <v-tab>{{ link.text }}
        </v-tab>
      </a>
    </v-tabs>
  </v-container>
</template>

<script>
export default {
  props: {
    toc: {
      type: Array,
      required: true
    }
  }
}
</script>
