<template>
  <div>
    <b-nav class="d-flex align-items-center justify-content-between">
      <b-navbar-brand class="d-flex align-items-center justify-content-start aling-">
        <img src="../assets/images/quiz_icon.png" class=" img d-inline-block align-top" alt="Kitten">
        QUIZ-APP
      </b-navbar-brand>
      <b-nav-item disabled>Counter: {{result}} \ {{totalQuestion}} </b-nav-item>
    </b-nav>
  </div>
</template>

<script>

export default {
  props: {
    result: Number,
    totalQuestion: Number,
  },
};
</script>

<style scoped>
.img{
  width: 50PX;
    height: 50PX;
}
</style>