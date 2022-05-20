<template>
  <div id="app">
    <h1 id="title">
      AgePredicter
    </h1>
    <input id="input" :value="this.name" @input="event => this.name = event.target.value" placeholder="What is your name?">
    <button id="button" @click="getData()"> {{ clickMsg }} </button>
    <div id="result">
      {{ resultText(click) }}
      <p> {{ this.ageMsg }} </p>
      <p> {{ this.genMsg }} </p>
      <p> {{ this.natMsg }} </p>
      <p> {{ this.armyMsg }} </p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'

Vue.prototype.$http = axios

var ohNo = false
var linkName = ''
var ageLink = 'https://api.agify.io/?name='
var natLink = 'https://api.nationalize.io/?name='
var genLink = 'https://api.genderize.io/?name='

export default {
  data: () => ({
    ohNo: false,
    clickMsg: 'Predict your name!',
    ageMsg: '',
    genMsg: '',
    natMsg: '',
    armyMsg: '',
    click: false,
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
        linkName = this.name //lokal variabel till name
        let ageResponse = await this.$http.get( //API-anrop för ålder
          ageLink + linkName
        )
        let genResponse = await this.$http.get( //API-anrop för könstillhörighet
          genLink + linkName
        )
        let natResponse = await this.$http.get( //API-anrop för nationalitet
          natLink + linkName
        )

        //få ut rätt värde från response, istället för ett helt dictionary
        this.age = ageResponse['data']['age']
        this.nationality = natResponse['data']['country'][0]['country_id']
        this.count = Math.round(ageResponse['data']['count'])
        this.natProbability = Math.round((natResponse['data']['country'][0]['probability']) * 100)

        if (genResponse['data']['gender'] == 'female') { //byt ut "female" och "male" mot "woman" och "man" för att få det mindre nedvärderande
          this.gender = 'woman'
        }
        else if (genResponse['data']['gender'] == 'male') {
          this.gender = 'man'
        }
        // if(this.gender && this.age && this.count && this.nationality){
        //   this.hideDiv(false)
        // }
        // else{
        //   this.hideDiv(true)
        // }
        this.click = true //knappen har blivit tryckt på
        this.ohNo = false
        return ageResponse, natResponse, genResponse //returnera ålder, nationalitet och könstillhörighet

      } catch (error) { //om det blir ett error, skriv det i terminalen (console) på hemsidan
        console.log(error)
        this.ohNo = true
        console.log(ohNo)
      }
    },
    resultText (click) { //Beroende på om knappen blivit klickad, skriv ut text
      if (click) { //om den blivit klickad på
        if (this.ohNo) {
          this.ageMsg = 'Something went wrong, try another name!',
          this.genMsg = ' ',
          this.natMsg = ' ',
          this.armyMsg = ' ',
          this.clickMsg = 'Try again!'
        }
        else {
          this.ageMsg = 'Your age is ' + this.age,
          this.genMsg = 'You are most likely a ' + this.gender,
          this.natMsg = 'Your country ID is: ' + this.nationality + ' with a probablitiy of: ' + this.natProbability + '%',
          this.armyMsg = 'Your ' + this.linkName + ' army would contain ' + this.count + ' soldiers.',
          this.clickMsg = 'Refresh!'
        }
      }
      else { //om den inte blivit klickad på
        this.ageMsg = 'Press the button to predict!',
        this.genMsg = ' ',
        this.natMsg = ' ',
        this.armyMsg = ' ',
        this.clickMsg = 'Predict your name!'
      }
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
  /* Hur ska sidan se ut, designmässigt */
  width: 100vw;
  height: 100vh;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: large;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color:blanchedalmond;
  color: black;
  background-image: url('C:/utveckling/agePredicter/website/src/assets/crowd.png');
  background-repeat: no-repeat;
  background-size: 100%;
  overflow: hidden;
}
/* Hur ska knappen se ut */
#button {
  color:rgb(22, 21, 18);
  background-color: rgb(145, 134, 117);
  /* border-color: aquamarine; */
  margin-left: 1%;
  margin-right: 5%;
  margin-top: 10%;
  margin-bottom: 5%;
  border-inline-width: 5px;
  border-block-width: 5px;
  border-inline-color: rgb(51, 48, 41);
  border-block-color: rgb(51, 48, 41);  
}
/* Hur ska input-rutan se ut */
#input {
  color: rgb(22, 21, 18);
  background-color: rgb(145, 134, 117);
  border-inline-width: 5px;
  border-block-width: 5px;
  border-inline-color: rgb(51, 48, 41);
  border-block-color: rgb(51, 48, 41);  
  margin-right: 1%;
  margin-left: 5%;
  margin-top: 5%;
  margin-bottom: 5%;
  outline: none;
}
/* Hur ska texten se ut */
#result {
  align-content: center;
  align-self: center;
  color:22, 21, 18;
  background-color: rgb(145, 134, 117);
  margin-top:5%;
  margin-left:30%;
  margin-right:30%;
  border-radius: 20%;
  text-align: center;
  padding: 5px;
  border-width: 5px;
  border-color: red;
}
#title {
  color:rgb(51, 48, 41);
  margin-right: 1000px;
}

/* #temp {
  color:aquamarine;
  background-color: aqua;
} */
</style>