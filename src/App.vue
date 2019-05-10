<template>
  <div id="app">
    <Header
      :correctAnswerCount="correctAnswerCount"
      :questionsCount="questions.length"
    />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
            v-if="currentQuestion"
            :currentQuestion="currentQuestion.question"
            :shuffledChoices="shuffledChoices"
            :questionIndex="questionIndex"
            :hasNextQuestion="hasNextQuestion"
            :nextQuestion="nextQuestion"
            :submitAnswer="submitAnswer"
            :isAnswerCorrect="isAnswerCorrect"
            />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import _ from 'lodash';

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      questionIndex: 0,
      correctAnswerCount: 0
    }
  },
  methods: {
    nextQuestion: function() {
      if (this.hasNextQuestion)
        this.questionIndex += 1;
    },
    submitAnswer: function (question, answer) {
      if (this.isAnswerCorrect(question, answer))
        this.correctAnswerCount += 1;
    },
    isAnswerCorrect: function (question, answer) {
      return _.find(this.questions, {question: question}).correct_answer === answer;
    } 
  },
  computed: {
    hasNextQuestion: function() {
      return this.questions.length - 1 > this.questionIndex;
    }, 
    shuffledChoices: function() {
      if(this.currentQuestion)
        return _.shuffle([...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]);
      else
        return [];
    },
    currentQuestion: function() {
      return this.questions[this.questionIndex];
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    })
      .then((response) => response.json())
      .then((questionsJson) => {
        this.questions = questionsJson.results;
        return this.questions;
      })
      .catch(() => { this.questions = [] });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
