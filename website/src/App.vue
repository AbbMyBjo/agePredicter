<template>
  <div id="app">
    <input :value="this.name" @input="event => this.name = event.target.value" placeholder="What is your name?">
    <button @click="getData()"> predict your name </button>
    <div id="result">
      <!-- <p> Your age is {{ age }}</p>
      <p> You are most likely a {{ gender }} </p>
      <p> Your country ID is {{ nationality }} with a probablitiy of {{ natProbability }}% </p>
      <p> Your {{ name }} army would contain {{ count }} soldiers </p> -->
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'

Vue.prototype.$http = axios

var linkName = ''
var ageLink = 'https://api.agify.io/?name='
var natLink = 'https://api.nationalize.io/?name='
var genLink = 'https://api.genderize.io/?name='

export default {
  data: () => ({
    buttonText: '',
    name: '',
    age: '0',
    count: '0',
    gender: '?',
    nationality: '?',
    linkName: '',
    natProbability: 0,
    ageLink: 'https://api.agify.io/?name=',
    natLink: 'https://api.nationalize.io/?name=',
    genLink: 'https://api.genderize.io/?name=',
    ageResponse: '',
    natResponse: '',
    genResponse: ''
  }),

  methods: {
    async getData () {
      try {
        linkName = this.name
        let ageResponse = await this.$http.get(
          ageLink + linkName
        )
        let genResponse = await this.$http.get(
          genLink + linkName
        )
        let natResponse = await this.$http.get(
          natLink + linkName
        )
        this.age = ageResponse['data']['age']
        this.nationality = natResponse['data']['country'][0]['country_id']
        this.count = Math.round(ageResponse['data']['count'])
        this.natProbability = Math.round((natResponse['data']['country'][0]['probability'])*100)

        if (genResponse['data']['gender'] == 'female'){
          this.gender = 'woman'
        }
        else if (genResponse['data']['gender'] == 'male'){
          this.gender = 'man'
        }  

        if(this.gender && this.age && this.count && this.nationality){
          this.hideDiv(false)
        }
        else{
          this.hideDiv(true)
        }

        return ageResponse, natResponse, genResponse

      } catch (error) {
        console.log(error)
      }
    },
    resultText (type){
      if (!input){
        text = 'Your age is' + this.age + 'You are most likely a' + this.gender + 'Your country ID is' + this.nationality + 'with a probablitiy of' + this.natProbability + 'Your' + this.name + 'army would contain' + this.count + 'soldiers'
      } //visa texten när input är 0 och annars visa ngot fint!!!!!
    }
    // hideDiv (show) {
    //   if (show){
    //     document.getElementById("result").style.display = 'none';
    //   }
    //   else{
    //     document.getElementById("result").style.display = 'block';
    //   }
    // }
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