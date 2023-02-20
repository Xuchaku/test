<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>]
    <Searcher></Searcher>
    <modal name="hello-world">
      hello, world!
    </modal>
    <button v-on:click="show"></button>
  </div>
</template>

<script>
import Searcher from "./components/Searcher.vue"
import VModal from 'vue-js-modal'
import Vue from 'vue'


Vue.use(VModal)
export default {
  name: 'App',
  components: {
    Searcher
  },
  methods: {
    show () {
      this.$modal.show('hello-world');
    },
    hide () {
      this.$modal.hide('hello-world');
    }
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
 
}
body{
  background-color: #ccc;
}
#app {

}
</style>
