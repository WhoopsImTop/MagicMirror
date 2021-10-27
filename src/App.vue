<template>
  <div id="app">
    <Clock class="top"/>
    <Weather class="right"/>
    <ChristmasCounter class="top" style="top: 40%"/>
    <Stundenplan class="left"/>
    <Todos class="bottom-left" />
  </div>
</template>

<script>
import Clock from './components/Clock.vue'
import Weather from './components/Weather.vue'
import ChristmasCounter from './components/ChrstmasCounter.vue'
import Stundenplan from './components/Stundenplan.vue'
import Todos from './components/Todos.vue'

export default {
  name: 'App',
  components: {
    Clock,
    Weather,
    ChristmasCounter,
    Stundenplan,
    Todos
  }
}
</script>

<style>
.top {
  position: absolute;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  top: 50px;
}

.left {
  position: absolute;
  left: 50px;
  top: 50px;
}

.right {
  position: absolute;
  right: 50px;
  top: 50px;
}

.bottom-right {
  position: absolute;
  bottom: 50px;
  right: 50px;
}

.bottom-left {
  position: absolute;
  bottom: 50px;
  left: 50px;
}

body {  
  background-color: #000000;
  position: relative;
  height: 98vh;
}
#app {
  font-family: Poppins, Helvetica, Arial, sans-serif;
}
</style>
