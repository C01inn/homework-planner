<template>
  <div id="app">
    <!-- heading -->
    <h3 id="titlew">Homework Planner</h3>

    <div id="homework">
      <Homework :work="work"/>
    </div>

  </div>
</template>
<script>
import Homework from '@/components/homework.vue'


export default {
  name: 'App',
  components: {
    Homework
  },
  data() {
    return {
    }
  }
}
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

#homework {
  width: 69%;
  margin-right: auto;
  margin-left: auto;
}
</style>
