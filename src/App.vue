<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <Header
            :resetQuiz="resetQuiz"
            :numCorrect="numCorrect"
            :numTotal="numTotal"
            :selectCategory="selectCategory"
          />
        </b-col>
      </b-row>
    </b-container>

    <b-container class="bv-example-row">
      <b-row>
        <b-col v-if="category" sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :resetQuiz="resetQuiz"
            :numCorrect="numCorrect"
            :numTotal="numTotal"
        /></b-col>
        <b-col v-else sm="6" offset="3" class="selectCat">
          Select A Category</b-col
        >
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/quiz/Header';
import QuestionBox from './components/quiz/QuestionBox';

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      category: null,
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    selectCategory(e) {
      if (e.target.innerHTML === 'Mythology') {
        this.category = 20;
        this.numTotal = 0;
        this.numCorrect = 0;
      } else if (e.target.innerHTML === 'History') {
        this.category = 23;
        this.numTotal = 0;
        this.numCorrect = 0;
      } else if (e.target.innerHTML === 'Animals') {
        this.category = 27;
        this.numTotal = 0;
        this.numCorrect = 0;
      } else if (e.target.innerHTML === 'Video Games') {
        this.category = 15;
        this.numTotal = 0;
        this.numCorrect = 0;
      }
      fetch(
        `https://opentdb.com/api.php?amount=10&category=${this.category}&difficulty=easy&type=multiple`,
        {
          method: 'get',
        },
      )
        .then(res => {
          return res.json();
        })
        .then(Questions => {
          this.questions = Questions.results;
        });
    },
    resetQuiz() {
      (this.index = 0), (this.numCorrect = 0);
      this.numTotal = 0;
    },
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
  },
  mounted: function() {
    fetch(
      `https://opentdb.com/api.php?amount=10&category=${this.category}&difficulty=easy&type=multiple`,
      {
        method: 'get',
      },
    )
      .then(res => {
        return res.json();
      })
      .then(Questions => {
        this.questions = Questions.results;
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
.selectCat {
  color: gray;
  font-size: 25px;
  margin-top: 20px;
}
</style>
