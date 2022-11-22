<template>
  <div>
    <!-- <lab-five/> -->
    <lab-six></lab-six>
    <!-- <lab-seven /> -->
    <!-- <lab-four /> -->
  </div>
</template>

<script>

// import LabFive from './components/LabFive.vue';
import LabSix from './components/LabSix.vue';
// import LabSeven from './components/LabSeven.vue';
// import LabFour from './components/LabFour.vue';
export default {
  name: 'App',
  components: {
    // LabFive
    LabSix
    // LabSeven
    // LabFour
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
</style>
