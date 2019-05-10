<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        <p class="question" v-html="currentQuestion.question"></p>
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item 
          v-html="choice"
          v-for="(choice, index) in shuffledAnswers" 
          :key="index"
          @click.prevent="!isAnswerSubmitted && setSelectedAnswer(choice)"
          :class="[
            !isAnswerSubmitted && choice ===  selectedAnswer ? 'selected-answer' :
            isAnswerSubmitted && choice === selectedAnswer && !isAnswerCorrect(currentQuestion, selectedAnswer) ? 'incorrect-answer' :
            isAnswerSubmitted && isAnswerCorrect(currentQuestion, choice) ? 'correct-answer' : ''
          ]"
        >
        </b-list-group-item>
      </b-list-group>

      <b-button 
        variant="primary" 
        :disabled="!selectedAnswer || isAnswerSubmitted" 
        @click.prevent="internalSubmitAnswer(currentQuestion, selectedAnswer)" 
      >
        Submit
      </b-button>
      <b-button 
        variant="success" 
        v-show="hasNextQuestion" 
        @click.prevent="nextQuestion()" 
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
    props: {
        currentQuestion: Object,
        hasNextQuestion: Boolean,
        nextQuestion: Function,
        submitAnswer: Function,
        isAnswerCorrect: Function
    },
    data() {
      return {
        selectedAnswer: null,
        shuffledAnswers: null,
        isAnswerSubmitted: false
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler() {
          this.selectedAnswer = null;
          this.isAnswerSubmitted = false;
          this.shuffledAnswers = this.getShuffledAnswers([...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]);
        }
      }
    },
    methods: {
      setSelectedAnswer: function(selectedAnswer) {
        this.selectedAnswer = selectedAnswer;
      },
      getShuffledAnswers: function(choices) {
        return _.shuffle(choices);
      },
      internalSubmitAnswer: function(question, answer) {
        this.submitAnswer(question, answer);
        this.isAnswerSubmitted = true;
      }
    },
    computed: {
      multipleChoices: function() {
        return [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      }
    }
}
</script>

<style scoped>
.question {
  height: 50px;
}

.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected-answer {
  background: lightblue;
}

.correct-answer {
  background: lightgreen;
}

.incorrect-answer {
  background: lightcoral;
}
</style>

