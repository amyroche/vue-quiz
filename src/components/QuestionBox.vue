<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">
      <b-list-group>
        <b-list-group-item
        v-for="(answer, index) in answers" 
        :key="index"
        @click="selectAnswer(index)"
        :class="answerClass(index)">{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered">Submit</b-button>

      <b-button 
      v-if="completed < 10"
      @click="nextQuestion" variant="success">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

  export default {
    props: {
      currentQuestion: Object,
      nextQuestion: Function,
      increment: Function
    },
    data() {
      return {
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false,
        completed: 0
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return answers
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler() {
          this.selectedIndex = null
          this.answered = false
          this.shuffleAnswers()
        }
      }
      // () {
      //   this.selectedIndex = null
      //   this.shuffleAnswers()
      // }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      shuffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
       // console.log(answers)
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      submitAnswer() {
        let isCorrect = false
        if(this.selectedIndex === this.correctIndex) {
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
        this.completed++
      },
      answerClass(index) {
        var answerClass = ''
        if (!this.answered && this.selectedIndex === index) {
          answerClass = 'selected'
        } else if (this.answered && this.correctIndex === index) {
          answerClass = 'correct'
        } else if (this.answered && this.selectedIndex === index && this.correctIndex != index) {
          answerClass = 'incorrect'
        } 
        return answerClass
      }
    }
  };
</script>

<style scoped>
  .question-box-container {
    max-width: 80%;
    margin: 0 auto;
  }

  .list-group {
    margin-bottom:15px;
  }

  .list-group-item:hover {
    background-color: #eee;
    cursor: pointer
  }

  .selected {
    background-color: lightblue;
  }

  .correct {
    background-color: lightgreen
  }

  .incorrect {
    background-color: lightcoral;
  }

  .btn {
    margin: 0px 10px;
  }
</style>