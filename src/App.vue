<template>
  <div id="app">
    <h1>Some example timer dots</h1>
    <timer-dot :border-width="3" color="blue" :size="50" :duration="5000" v-on:timerCompleted="restart"></timer-dot>
    <timer-dot color="rgb(255, 0, 255)" :duration="15000" v-on:timerCompleted="restart"></timer-dot>
    <h2>nllptr @</h2>
    <ul>
      <li><a href="https://twitter.com/nllptr" target="_blank">twitter</a></li>
      <li><a href="https://github.com/github" target="_blank">github</a></li>
    </ul>
  </div>
</template>

<script>
import TimerDot from './TimerDot.vue'

export default {
  name: 'app',
  data () {
    return {
      timer1: 5000
    }
  },
  components: {
    TimerDot
  },
  methods: {
    restart: function(component) {
      component.restart()
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
