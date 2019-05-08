<template>
  <div id="app">
    <Header
      :questionIndex="questionIndex"
      :questionsCount="questions.length"
    />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
            v-if="questions[questionIndex]"
            :currentQuestion="questions[questionIndex]"
            :questionIndex="questionIndex"
            :hasPreviousQuestion="hasPreviousQuestion"
            :hasNextQuestion="hasNextQuestion"
            :nextQuestion="nextQuestion"
            :previousQuestion="previousQuestion"
            />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      questionIndex: 0
    }
  },
  methods: {
    hasNextQuestion: function() {
      return this.questions.length - 1 > this.questionIndex;
    },
    hasPreviousQuestion: function() {
      return this.questionIndex > 0;
    },
    nextQuestion: function() {
      if (this.hasNextQuestion())
        this.questionIndex += 1;
    },
    previousQuestion: function() {
      if (this.hasPreviousQuestion())
        this.questionIndex -= 1;
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
      .catch(() => this.questions = []);
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
