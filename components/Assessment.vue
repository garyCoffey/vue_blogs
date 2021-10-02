<template>
  <v-container fluid>
    {{ (score/questions.length) *100 }}%
    <v-form
      ref="form"
      @submit.prevent="submit"
    >
      <v-container
        v-for="(question, i) in questions"
        :key="i"
        fluid
      >
        <v-radio-group
          v-model="assessmentConfigs[`question${i}`]"
        >
          <template v-slot:label>
            <div>{{ question.question }}</div>
          </template>

          <v-radio
            v-for="option in getOptions(question)"
            :key="option.id"
            :value="option"
          >
            <template v-slot:label>
              <div>{{ option }}</div>
            </template>
          </v-radio>
        </v-radio-group>
      </v-container>
      <VBtnUi
        id="assessment-submit"
        name="assessment-submit"
        class="mr-4"
        type="submit"
        :disabled="false"
        button-title="Submit"
      />
    </v-form>
  </v-container>
</template>

<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true
    }
  },
  data () {
    const score = 0
    const assessmentConfigs = {}
    return {
      score,
      assessmentConfigs
    }
  },
  mounted () {
    this.questions.forEach((_, i) => (this.assessmentConfigs[`question${i}`] = ''))
  },
  methods: {
    getOptions (question) {
      return [...question.incorrect_answers, question.correct_answer].sort((_a, _b) => 0.5 - Math.random())
    },
    submit (e) {
      e.preventDefault()
      this.questions.forEach((question, i) => {
        if (this.assessmentConfigs[`question${i}`] === question.correct_answer) { this.score++ }
      })
    }
  }
}
</script>
<style scoped>
</style>
