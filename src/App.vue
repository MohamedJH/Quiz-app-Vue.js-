<template>
  <div id="app">
    <b-container class="bv-example-row border border-dark">
      <b-row class=" justify-content-md-start ustify-content-md-start">
        <b-col class="col-md-6 col-sm-10 border border-dark m-1">
          <Header :result="result" :totalQuestion="totalQuestion" />
        </b-col>
      </b-row>

      <b-row class="justify-content-md-center">
        <b-col class="col-md-6 col-sm-10 border border-dark mt-3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :totalOfCorrectAnswers="totalOfCorrectAnswers"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox";
import "./assets/stylesheets/Main.css";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      result: 0,
      totalQuestion: 0,
    };
  },
  methods: {
    next() {
      this.questions.length - 1 > this.index
        ? this.index++
        : alert("End Of The Game");
    },
    totalOfCorrectAnswers(isCorrect) {
      this.totalQuestion++;
      isCorrect === true ? this.result++ : null;
      this.result;
    },
  },

  mounted: function() {
    return fetch(
      "https://opentdb.com/api.php?amount=10&category=27&type=multiple",
      { method: "get" }
    )
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
