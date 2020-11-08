<template>
  <div class="context">
    <div>
      <DateComponent></DateComponent>
    </div>
    <!-- {{msg}} -->
  </div>
</template>

<script>
  import DateComponent from "./DateComponent.vue"
  export default {
    name: 'Index',
    components: {
      DateComponent
    },
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        now: new Date()
      }
    }
  }
</script>

<style scoped>
  div {
    margin: 0px;
    padding: 0px;
  }
</style>
