<template
  slot
  name="trivia"
>
  <v-container fluid>
    <p v-if="$fetchState.pending">
      Fetching questions...
    </p>
    <div v-else>
      <Assessment :questions="questions.results" />
    </div>
  </v-container>
</template>

<script>
export default {
  async fetch () {
    this.questions = await fetch(
      'https://opentdb.com/api.php?amount=10'
    ).then(res => res.json())
  },
  data () {
    return {
      questions: []
    }
  }

}
</script>

<style>
</style>
