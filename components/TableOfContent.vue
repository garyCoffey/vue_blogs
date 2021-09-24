<template>
  <v-sheet
    v-if="toc.length > 3"
  >
    <h3>Table of Content</h3>
    <v-container id="toc" fluid>
      <v-tabs
        v-for="link of toc"
        :key="link.id"
        :class="{ toc2: link.depth === 2, toc3: link.depth === 3 }"
      >
        <!-- <a

        > -->
          <v-tab
          href
          >{{ link.text }}</v-tab>
        <!-- </a> -->
      </v-tabs>
    </v-container>
  </v-sheet>
</template>

<script>
export default {
  props: ['toc'],
}
</script>