<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        <p class="question" v-html="currentQuestion"></p>
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item 
          v-html="choice"
          v-for="(choice, index) in shuffledChoices" 
          :key="index"
          @click.prevent="!isAnswerSubmitted && setSelectedAnswer(choice)"
          :class="setChoiceClass(choice)"
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
export default {
    props: {
        currentQuestion: String,
        shuffledChoices: Array,
        hasNextQuestion: Boolean,
        nextQuestion: Function,
        submitAnswer: Function,
        isAnswerCorrect: Function
    },
    data() {
      return {
        selectedAnswer: null,
        isAnswerSubmitted: false
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler() {
          this.selectedAnswer = null;
          this.isAnswerSubmitted = false;
        }
      }
    },
    methods: {
      setSelectedAnswer: function(selectedAnswer) {
        this.selectedAnswer = selectedAnswer;
      },
      internalSubmitAnswer: function(question, answer) {
        this.submitAnswer(question, answer);
        this.isAnswerSubmitted = true;
      },
      setChoiceClass(choice) {
        if (!this.isAnswerSubmitted && choice === this.selectedAnswer)
          return 'selected-answer';
        else if (this.isAnswerSubmitted && choice === this.selectedAnswer && !this.isAnswerCorrect(this.currentQuestion, this.selectedAnswer))
          return 'incorrect-answer';
        else if (this.isAnswerSubmitted && this.isAnswerCorrect(this.currentQuestion, choice))
          return 'correct-answer'
        else 
          return '';
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

