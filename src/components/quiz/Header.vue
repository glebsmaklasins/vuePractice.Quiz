<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Quiz</b-nav-item>
      <b-nav-item disabled>{{ numCorrect }}/{{ numTotal }}</b-nav-item>
      <b-nav-item @click="resetQuiz">Reset Quiz</b-nav-item>
      <br />
    </b-nav>
    <b-nav tabs>
      <b-nav-item @click="selectCategory">Mythology</b-nav-item>
      <b-nav-item @click="selectCategory">History</b-nav-item>
      <b-nav-item @click="selectCategory">Animals</b-nav-item>
      <b-nav-item @click="selectCategory">Video Games</b-nav-item>
      <br />
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ['numTotal', 'numCorrect', 'resetQuiz', 'selectCategory'],
};
</script>
