<template>
  <div class="question-box-container">
  <b-jumbotron>
    <template v-slot:lead>
      {{ currentQuestion.question }}
    </template>

    <hr class="my-4">

    <b-list-group>
        <b-list-group-item 
        v-for="(answer, index) in answers" 
        :key="index"
        @click="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : '']"
        >
          {{ answer }}
        </b-list-group-item>   
    </b-list-group>
    
    <p></p>
    <b-button 
    variant="primary"
    @click="submitAnswer"
    >
      Submit
    </b-button>
    <b-button variant="success" href="#" @click="next">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
    selectedIndex: null,
    shuffledAnswers: []
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
      this.selectedIndex = null
      this.shuffleAnswers()
      }
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  } ,
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      let isCorrect = false

      if (this.selectedIndex === this.correctIndex){
        isCorrect = true
      }
          this.increment(isCorrect)
    }
  }

}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: blueviolet;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>