<template
  slot
  name="trivia"
>
  <v-container fluid>
    <p v-if="$fetchState.pending">Fetching questions...</p>
    <div v-else>
      <Assessment :questions="questions.results"/>
    </div>
  </v-container>
</template>

<script>
  import { defineComponent } from '@vue/composition-api'
  export default defineComponent({
    data() {
      return {
        questions: []
      }
    },
    async fetch() {
      this.questions = await fetch(
        'https://opentdb.com/api.php?amount=10'
      ).then(res => res.json())
    }

  })
</script>

<style>
</style>
