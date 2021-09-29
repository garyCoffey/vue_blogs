<template>
  <v-container fluid>
    {{(score/questions.length) *100}}%
    <v-form
      ref="form"
      @submit.prevent="submit"
    >
      <v-container
        fluid
        v-for="(question, i) in questions"
        v-bind:key="i"
      >
        <v-radio-group
          v-model="assessmentConfigs[`question${i}`]"
        >
          <template v-slot:label>
            <div v-html="question.question" />
          </template>

          <v-radio
            v-for="option in getOptions(question)"
            v-bind:key="option.id"
            :value="option"
          >
            <template v-slot:label>
              <div v-html="option" />
            </template>
          </v-radio>
        </v-radio-group>
      </v-container>
      <v-btn
        class="mr-4"
        type="submit"
        :disabled="false"
      >
        Submit
      </v-btn>
    </v-form>
  </v-container>
</template>

<script>
  export default {
    data() {
      let score = 0
      let assessmentConfigs = {}
      return {
        score,
        assessmentConfigs
      }
    },
    mounted() {
      this.questions.forEach((_, i) => this.assessmentConfigs[`question${i}`] = '')
    },
    props: {
      questions: {
        type: Array,
        required: true
      }
    },
    methods: {
      getOptions: function(question) {
        return [...question.incorrect_answers, question.correct_answer].sort((a, b) => 0.5 - Math.random());
      },
      submit (e) {
        e.preventDefault();
        this.questions.forEach((question, i) => {
          if (this.assessmentConfigs[`question${i}`] === question.correct_answer) this.score++
        })
      },
    }
  }
</script>
<style scoped>
</style>