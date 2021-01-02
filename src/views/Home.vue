<template>
  <!--
    We will be using the Pure CSS framework (https://purecss.io/)
    Vue is very "greedy" when it comes to DOM manipulation, so a pure
    CSS framework without and javascript is what we want.
  -->
  <div class="pure-g">
    <div class="pure-u-2-24">
    </div>
    
    <div class="pure-u-20-24">
      <h1> JavaScript Knowledge Quiz</h1>

      <!-- 
        This is the main form populated from the API and used to gather user's responses.
        We need to overwrite the default form handler with our own onSubmit().
        Otherwise, the form will post in itself and we will lose the state of our internal data structures.
      -->
      <form id="quizForm" class="pure-form" @submit.prevent="onSubmit">
        <fieldset  v-for="question in questions" :key="question._id" :id="question._id">
          <legend>
            {{question.query}} 
            <span v-if="responses[question._id].incorrect" class="error-tag">&#x292B;</span>
          </legend>
          <label v-for="choice in question.choices" :key="choice.key" :for="choice._id" class="pure-radio">
            <input @change="onChange" v-model="responses[question._id].selected" type="radio"
              :id="choice._id" :name="question._id" :value="choice.key" /> {{choice.description}}
          </label>
        </fieldset>
        <hr/>
        <p class="buttons">
          <input class="pure-button pure-button-primary" type="submit" value="Submit">
        </p>
      </form>

      <div class="vertical-space"></div>

      <!--
        This section will be removed from DOM when user makes changes.
        It will be added to DOM again once they hit Submit to recalculate the results.

        This is a "fake" form, just to reuse the nice CSS classes we defined.
        It doesn't actually any interactive controls to trigger submission,
        but we are still going to prevent submission just in case.
      -->
      <form v-if="resultsReady" class="pure-form" v-on:submit.prevent="">
        <fieldset class="summary">
          <legend>Your Results!</legend>
          <span class="results results-total" v-if="totalQuestions"> Total questions: {{totalQuestions}} </span>
          <span class="results results-correct" v-if="correctAnswers"> Correct answers: {{correctAnswers}} </span>
          <span class="results results-incorrect" v-if="incorrectAnswers"> Inorrect answers: {{incorrectAnswers}} </span>
        </fieldset>
      </form>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Home',

  // Initialize reactive data properties
  // Vue needs these to be happy
  data() {
    return {
      questions: {},
      responses: {},
      err: null,
      api: "http://192.168.0.13:5000/api/v1/questions",
      totalQuestions: null,
      correctAnswers: null,
      incorrectAnswers: null,
      resultsReady: false,
    }
  },

  methods: {
    // Pull the list of questions from the backend API using axios
    // and initialize questions and responses objects.
    // We need a separate response object to keep track of which
    // option the user selected, which option is the correct one,
    // and finally whether they answered incorrectly or not.
    loadData() {
      this.err = false;
      axios
        .get(this.api)
        .then(res => {
          this.questions = res.data.data;
          console.log(this.questions);
          this.totalQuestions = Object.keys(this.questions).length;
          this.questions.forEach(question => {
            this.responses[question._id] = {
              correct: question.correct,
              selected: null,
              incorrect: false
            }
          });
        })
        .catch(err => {
          this.err = true;
        });
    },

    // Hide results when user makes a different selection
    // We'll have to recalculate them when they hit submit again
    onChange() {
      this.resultsReady = false;
    },

    // (Re)calculate results (correct and incorrect questions) when user hits Submit
    onSubmit() {
      this.correctAnswers = 0;
      var questionID;
      for (questionID in this.responses) {
        if (this.responses[questionID].correct === this.responses[questionID].selected) {
          this.responses[questionID].incorrect = false
          this.correctAnswers += 1;
        } else {
          this.responses[questionID].incorrect = true
        }
      }
      this.incorrectAnswers = this.totalQuestions - this.correctAnswers;

      this.resultsReady = true;
    }
  },

  // mounted() hook is called when Vue app is mounted
  mounted() {
    this.loadData()
  }
}
</script>

<style>

h1 {
  text-align: center;
}

fieldset {
  padding-bottom: 10px;
}

label {
  padding-left: 10px;
}

.pure-form {
  border-style: solid;
  border-color: lightgray;
}

.pure-form legend {
  background-color: gray;
  padding-left: 10px;
  font-weight: bold;
  color: white;
}

.buttons {
  text-align: center;
}

.vertical-space {
  padding-bottom: 10px;
}

.error-tag {
  margin: 0 0.1em;
  padding: 0.1em 0.3em;
  color: white;
  background: orangered;
}

.results {
  margin: 0 0.1em;
  padding: 0.3em 1em;
}

  .results-total {
    background: lightskyblue;
  }

  .results-correct {
    background: lightgreen;
  }

  .results-incorrect {
    background: lightsalmon;
  }


.summary {
  text-align: center;
}

</style>