<template>
  <div id="app">
    <input v-model="text" placeholder="What is your name?">
    <button @click="getData()"> Predict your age </button>
    <p>{{ response }}</p>
  </div>
</template>

<!-- API: anrop https://api.agify.io/?name=valfrittnamn -->

<script>
import axios from 'axios';
import Vue from 'vue';

Vue.prototype.$http = axios;

var posts = '';
var text = '';
var link = 'https://api.agify.io/?name=';

export default {
  data: () => ({
    posts: '',
    text: '',
    link: 'https://api.agify.io/?name=',
    response: ''
    }),

  methods: {
    async getData() { //Gör en get-request till API:n och få tillbaka en dictionary
      try {
        let response = await this.$http.get(
          String(link+text+"/name")
        );
        // JSON responses are automatically parsed.
        this.posts = response.data;
        console.log(posts);
        console.log(response);
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #037b77;
  color: black;
  margin-top: 60px;
}
</style>
