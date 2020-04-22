<template>
  <div id="app">
    <Header 
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="10" md="12" offset-sm="1" offset-md="0">
          <QuestionBox 
            v-if="questions.length" 
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :index="index"
          />
          <!-- The v-if prevents an error occurring when Vue tries to render an empty questions array -->
        </b-col>
      </b-row>
    </b-container>
    <p align="center">Created using Vue and Bootstrap</p>
    <img alt="Vue logo" src="./assets/logo.png" style="height: 50px; width: auto; margin-bottom: 12px;">
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
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
    next(){
      this.index++
    },
    increment(isCorrect){
      if (isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=20&category=17&type=multiple', {
      method: 'get'
    })
    .then((response) => {
      return response.json()
    })
    .then((jsonData) => {
      this.questions = jsonData.results;
      // console.log(this.questions);
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 35px;
}
</style>
