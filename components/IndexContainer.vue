<template>
  <v-container fluid>
    <v-row align="stretch" class="ma-2">
      <v-col
        v-for="item of items"
        :key="item.slug"
        md="4"
        class="pa-3 d-flex flex-column"
      >
        <ContentCard
          :title="item.title"
          :description="item.description"
          :img="item.img ? item.img : '/defaultPic.png'"
          :alt="item.alt ? item.alt : 'VSCode screenshot'"
          :date="item.updatedAt > item.createdAt ? item.updatedAt : item.createdAt"
          :to="{ name: `${item.type}-slug`, params: { slug: item.slug } }"
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
