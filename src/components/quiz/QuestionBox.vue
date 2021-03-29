<template>
  <div>
    <b-jumbotron v-if="numTotal === 10 && answered">
      <template #lead>
        Congratulations!!!
        <br />
        Your Result is
      </template>
      <hr class="my-4" />
      <p>{{ numCorrect }}/{{ numTotal }}</p>

      <b-button variant="primary" @click="resetQuiz">Try Again</b-button>
    </b-jumbotron>
    <b-jumbotron v-else>
      <template #lead>
        {{ cT }}
      </template>
      <hr class="my-4" />
      <p>
        <b-list-group>
          <b-list-group-item
            v-for="(answer, index) in shuffledAnswers"
            :key="index"
            @click.prevent="selectAnswer(index)"
            :class="[answerClass(index)]"
            >{{ answer }}</b-list-group-item
          >
        </b-list-group>
      </p>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
        >submit</b-button
      >
      <b-button
        @click="
          next();
          correctText();
        "
        :disabled="!answered"
        variant="success"
        href="#"
        >next question</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    resetQuiz: Function,
    numCorrect: Number,
    numTotal: Number,
  },
  data() {
    return {
      cT: this.correctText(),
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false,
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.answered = false;
        this.correctText();
      },
    },
  },
  methods: {
    correctText() {
      let cT = this.currentQuestion.question;

      let x = cT
        .replace(/&quot;/g, '"')
        .replace(/&#039;/g, "'")
        .replace(/&rsquo;/g, '’')
        .replace(/&amp;/g, '&')
        .replace(/&Uuml;/g, 'Ü')
        .replace(/&eacute;/g, 'é');
      console.log(x);
      this.cT = x;
    },
    answerClass(index) {
      let answerClass = '';

      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected';
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct';
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = 'incorrect';
      }
      return answerClass;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.answered = true;
    },
    selectAnswer(index) {
      if (!this.answered) {
        this.selectedIndex = index;
      }
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      let checkedAnswers = [];
      answers.forEach(answer => {
        checkedAnswers.push(
          answer
            .replace(/&quot;/g, '"')
            .replace(/&#039;/g, "'")
            .replace(/&rsquo;/g, '’')
            .replace(/&amp;/g, '&')
            .replace(/&Uuml;/g, 'Ü')
            .replace(/&eacute;/g, 'é'),
        );
      });
      this.shuffledAnswers = _.shuffle(checkedAnswers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer,
      );
    },
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15 px;
}
.list-group-item:hover {
  background-color: #eeee;
  cursor: pointer;
  margin-bottom: 15 px;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: lightcoral;
}
</style>
