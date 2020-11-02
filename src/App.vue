<template>
  <ExpenseTracker/>
</template>

<script>
import ExpenseTracker from './components/expense-tracker.vue'

export default {
  name: 'App',
  components: {
    ExpenseTracker
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
</style>
