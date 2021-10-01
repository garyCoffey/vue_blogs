<template>
  <v-container fluid >
    <v-row align="stretch" class="ma-2">
      <v-col
        md="4"
        class="pa-3 d-flex flex-column"
        v-for="item of items"
        :key="item.slug"
      >
        <ContentCard
          v-bind:title="item.title"
          v-bind:description="item.description"
          v-bind:img="item.img ? item.img : '/defaultPic.png'"
          v-bind:alt="item.alt ? item.alt : 'VSCode screenshot'"
          v-bind:date="item.updatedAt > item.createdAt ? item.updatedAt : item.createdAt"
          v-bind:to="{ name: `${item.type}-slug`, params: { slug: item.slug } }"
        />
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
  export default {
    props: {
      items: {
        type: Array,
        required: true
      }
    }
  }
</script>
