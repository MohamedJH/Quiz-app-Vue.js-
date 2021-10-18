<template>
  <div class="">
    <b-jumbotron fluid>
      <template #lead> {{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          :disabled="isAnswered===true"
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="[
            !isAnswered && selectedIndex === index
              ? 'selected'
              : isAnswered && correctIndex === index
              ? 'success'
              : isAnswered && selectedIndex === index && correctIndex !== index
              ? 'failure'
              : '',
          ]"
        >
          {{ answer }}
           <b-icon v-if="[!isAnswered && selectedIndex === index]" icon="" scale="2" variant=""></b-icon>
           <b-icon v-else-if="[isAnswered && correctIndex === index]" icon="check2" scale="2" variant="success"></b-icon>
           <b-icon v-else-if="[isAnswered && selectedIndex === index && correctIndex !== index]" icon="x" scale="2" variant="danger"></b-icon>
           <b-icon v-else icon="" scale="2" variant=""></b-icon>
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click.prevent="submitAnswer"
        :disabled="selectedIndex === null || isAnswered"
      >
        Submit
      </b-button>
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    totalOfCorrectAnswers: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      isAnswered: false,
    }
  },
  /*computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },*/
  watch: {
    currentQuestion (){
      this.selectedIndex = null;
      this.correctIndex = null, 
      this.isAnswered = false;
      this.handle_shuffledAnswers();
    },
  },
  mounted() {
    this.handle_shuffledAnswers();
  },

  methods: {
    selectAnswer(index) {
      
      this.selectedIndex = index;
      console.log(this.selectedIndex);
    },
    submitAnswer() {
      let isCorrect;
      this.selectedIndex === this.correctIndex
        ? (isCorrect = true)
        : (isCorrect = false);
      this.isAnswered = true;
      this.totalOfCorrectAnswers(isCorrect);
    },
    handle_shuffledAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.success {
  border: 2px solid green;
  display: inline-flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
  align-items: baseline;
}
.failure {
  border: 2px solid red;
  display: inline-flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
  align-items: baseline;
}
.nostate {
  border: none;
  float: left;
}
.selected {
  background-color: rgba(243, 232, 232, 0.781);
  border-color: 2px solid grey;
}
.btn {
  margin: 0 5px;
}
</style>
