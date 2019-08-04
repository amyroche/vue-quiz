<template>
  <div id="app">
    <Header 
    :numCorrect="numCorrect"
    :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row sm="6">
        <b-col><QuestionBox 
          v-if="questions.length && questions.length <= 10"
          :currentQuestion="questions[index]"
          :nextQuestion="nextQuestion"
          :increment="increment" /></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
  import Header from './components/Header.vue'
  import QuestionBox from './components/QuestionBox.vue'

  export default {
    name: 'app',
    components: {
      Header,
      QuestionBox
    },
    data() { 
      return {
        questions: [],
        index: 0,
        numCorrect: 0,
        numTotal: 0
      }
    },
    methods: {
      nextQuestion() {
        this.index++
      },
      increment(isCorrect) {
        if(isCorrect) {
          this.numCorrect++
        }
        this.numTotal++
      }
    },
    mounted: function() {
        fetch('https://opentdb.com/api.php?amount=10&category=12&type=multiple', {
          method: 'get'
        }).then((response) => {
          return response.json()
        }).then((jsonData) => {
          //JSON.stringify(jsonData.results)
          this.questions = jsonData.results
        })
      }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>