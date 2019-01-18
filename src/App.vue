<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab">
      i am tab
    </div>
    <div class="content">
      i am content
    </div>
    <!--<router-view/>-->
  </div>
</template>

<script>
  import vHeader from './components/header/header'
export default {
  name: 'App',
  components: {
    'v-header': vHeader
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
</style>
