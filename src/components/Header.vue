<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{questionIndex + 1}}/{{questionsCount}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    questionIndex: Number,
    questionsCount: Number
  }
}
</script>