<template>
  <div id="app">
    <MemeGenerator />
  </div>
</template>

<script>
import MemeGenerator from './components/MemeGenerator.vue';

export default {
  name: 'App',
  components: {
    MemeGenerator,
  },
};
</script>

<style>
@import './style.css';
</style>