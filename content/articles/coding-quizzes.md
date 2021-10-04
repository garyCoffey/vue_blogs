---
title: VUE Coding Triva Mark I
description: Coding a trivia game in Vue
img: /thumbnails/exam.webp
alt: exam
type: blog
tags: [blogs, code, vue, game]
categories: [blogs, code, vue, game]
---

### Trivia Time

<v-btn-ui id="quiz-game" name="quiz-game" nuxt link title="Go To Game" to="/game/trivia">
  <template #v-btn-ui>
  </template>
</v-btn-ui>

Today we will go over how to code a simple trivia game in Vue. This is the first version of Trivia, so all links will be permalinks because I expect to revisit this game in the future to do some updates. As it stand, the UI is fairly basic for reasons we will soon get into.

Making a trivia game was pretty fun and is a good challenge for anyone newer to software engineering in my experience. Much of what this game involves is very similar to my everyday job. A trivia game, at its core, is just a form that submits data to be processed.

The reason this game is very basic in this first version is that I want to create multiple quiz-type games. Looking at different quiz types it was hard to imagine how generic my assessment component needed to be to support different scoring, questions, and assessment completions. For that reason, we are keeping this code basic, so when we code out a new quiz game, it will help make the generic parts more apparent. I am a fan of putting of design decisions if possible so code doesn't get overly complex based on what *may* be needed vs what ***will*** be needed.

Since I expect to update the code someday, here are a couple of links to the code as of the time this is being written:

- [Triva Component](https://github.com/garyCoffey/vue_blogs/blob/e530efe09cf51538c7eabbef9d91fc3d5adfb993/components/global/games/Trivia.vue#L1-L32)

- [Assessment Component](https://github.com/garyCoffey/vue_blogs/blob/e530efe09cf51538c7eabbef9d91fc3d5adfb993/components/Assessment.vue#L1-L76)

Okay so this is the process we are going to follow today (or night/evening/afternoon/ w/e):

- [Getting Questions](#getting-questions)
- [Showing Questions](#showing-questions)
- [Scoring](#scoring)
- [Future Updates](#future-updates)

I told you this one would be simple!

### Getting Questions

One of the first challenges I faced was to find a way to get questions. I decided to use a third party API for this to keep this part a little easier. [I found a very simple API for the job](https://opentdb.com/api_config.php).  Here is my code that accesses this API and sends the results to my assessment component:

```vue
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
```

In the above, you can see it is a pretty simple chunk of code that just sets the results of the trivia API call to a data piece called questions. We then pass that data to the assessment component. Here is an example of what that API returns:

```json
  "results": [
    {
      "category": "Entertainment: Film",
      "type": "multiple",
      "difficulty": "hard",
      "question": "Which was the first of Alfred Hitchcock&#039;s movies to be filmed in colour?",
      "correct_answer": "Rope",
      "incorrect_answers": [
        "Psycho",
        "Vertigo",
        "Rebecca"
      ]
    }
]
```

### Showing Questions

So after sending that data piece to the assessment component, the first job of that component is to present the questions to the gamer. 

#### Assessment Component

```vue
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
        title="Submit"
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
```

In the above, to present questions we are using a form. In the form we do a for loop over the given questions. For each question, we print the question for the gamer, and then do a for loop to over the possible answers using radio buttons. There is some other stuff going aside from presentation. In the script we take the given data, mix the correct answer with the incorrect answer and shuffle them up a bit to make the options work. We also give question a key and shove it into `assessmentConfigs` to use this for our v-model. We will cover that more soon in the scoring section. 

 ### Scoring

In the [above code](#assessment-component) there are a few key pieces built for scoring. One of them is setting the `v-model` value on the radio groups so Vue knows what value was given for each question.  The other major piece is here in the scoring that happens on submit:

```vue
    submit (e) {
      e.preventDefault()
      this.questions.forEach((question, i) => {
        if (this.assessmentConfigs[`question${i}`] === question.correct_answer) { this.score++ }
      })
    }
```
When a user clicks the submit button, we loop over the originally given questions array. For each question in the array, we check the `assementConfigs` at the index (which should match) and see if the given answer is the correct answer. If so we bump up the score data piece and, if not, we do nothing. 

The last piece of scoring is just to display the score to the gamer. To do this we just render the current score at the top of the form:

```vue
    {{ (score/questions.length) *100 }}%
```

### Future Updates

I want to harken back to the original statement that this version is meant to be really basic. That means that there are a lot of updates that can be done. I might change the game to score as users answer questions to give more immediate feedback. I held off on this for now, because the whole scoring piece might live in its own component. I have an idea of maybe display like gifs that change depending on the score... Not really sure, but I didn't want to make that decision here.

Another idea I had was to also do something more like personality quizzes. These would share the assessment component and would also drive changes to presenting questions/scoring. The scoring might not be right/wrong but adding weight to some end answer. 

The other major updates will probably be to the UI in general. The API I am using is sending back some raw html. At first I used `v-html` in some of the Vue components but removed it to prevent possible cross site scripting attacks. I might swap out the API or look into other ways of safely presenting the info.  I would also like to disable the submit button until all of the answers are in (unless I do the score as you go which would make the submit button useless). 

### Conclusion

I believe that is about it.... This wasn't too hard of a game to build since it is done very basically. Hopefully I will have Mark II post coming soon to show off some other cool updates. I think it will also be good to show off how the components will adapt for multiple quiz types. 