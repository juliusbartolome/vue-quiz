<template>
  <header>
    <b-nav tabs>
      <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{correctAnswerCount}}/{{questionsCount}}</b-nav-item>
    </b-nav>
  </header>
</template>

<script>
export default {
  props: {
    correctAnswerCount: Number,
    questionsCount: Number
  }
}
</script>

<style scoped>
header {
  margin-bottom: 10px;
}
</style>
