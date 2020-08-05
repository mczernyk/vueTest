<template>
  <div class="boxComponent">
    <b-jumbotron>
      <template v-slot:lead>
        {{ questionNow.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{answer}}

        </b-list-group-item>

      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button
        @click="next"
        variant="success"
        :disabled="selectIndex === null || !answered"
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    questionNow: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  computed: {
    answers(){
      let answers = [...this.questionNow.incorrect_answers]
      answers.push(this.questionNow.correct_answer)
      return answers
    }
  },
  watch: {
    questionNow: {
      // immediate mounts on first render as prop instead of just refresh/update
      immediate: true,
      handler() {
        this.selectIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    },
  // can use function below instead of questionNow object + mounted(){this.shuffleAnswers()} to mount on initial render
  //   questionNow(){
  //     this.selectIndex = null
  //     this.shuffleAnswers()
  //   }
  },
  methods: {
    selectAnswer(index){
      this.selectIndex = index
    },
    shuffleAnswers(){
      let answers = [...this.questionNow.incorrect_answers, this.questionNow.correct_answer]
      this.shuffledAnswers =_.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.questionNow.correct_answer)
    },
    submitAnswer(){
      let isCorrect = false

      if (this.selectIndex === this.correctIndex){
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },
    answerClass(index){
      let answerClass = ''
      if (!this.answered && this.selectIndex === index){
        answerClass = 'selected'
      } else if ( this.answered && this.correctIndex === index){
        answerClass = 'correct'
      } else if (this.answered && this.selectIndex === index && this.correctIndex !== index){
        answerClass = 'incorrect'
      }
      return answerClass
    }
  },
  // can use mounted() + function in 'watch' to mount on initial render
  // mounted() {
  //   this.shuffleAnswers()
  // }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
  }
  .list-group-item:hover {
    background: #EEE;
    cursor: pointer;
  }
  .btn {
    margin: 15px
  }
  .selected {
    background-color: yellow;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: red;
  }
</style>
