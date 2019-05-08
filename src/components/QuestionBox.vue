<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        <p v-html="currentQuestion.question"></p>
      </template>

      <hr class="my-4">

      <p>
          List of answers
      </p>

      <b-button variant="primary">Submit</b-button>
      <b-button v-show="hasPreviousQuestion()" @click="previousQuestion()">Previous</b-button>
      <b-button variant="success" v-show="hasNextQuestion()" @click="nextQuestion()">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
    props: {
        currentQuestion: Object,
        hasPreviousQuestion: Function,
        hasNextQuestion: Function,
        nextQuestion: Function,
        previousQuestion: Function
    }
}
</script>