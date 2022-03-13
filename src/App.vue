<template>
  <div id="app">
    <div class="layout">
      <div class="date-filter">
        <Datepicker value="2019-11-22"></Datepicker>
      </div>
      <RepositoryItem></RepositoryItem>
    </div>
  </div>
</template>

<script>
import RepositoryItem from './components/RepositoryItem.vue'
import Datepicker from 'vuejs-datepicker';

export default {
  name: 'App',
  components: {
    RepositoryItem,
    Datepicker
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 900px;
  margin: 0 auto;
}
.layout {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-top: 60px;
}
.date-filter {
  align-self: flex-end;
  margin-bottom: 30px;
}
</style>
