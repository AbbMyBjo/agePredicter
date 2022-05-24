<template>
  <div id="div">
    <!-- Välj ett unikt id till varje komponent för att styla -->
    <button id="title" @click="reload()">
      AgePredicter
    </button>
    <br>
    <input id="input" :value="this.name" @input="event => this.name = event.target.value"
      placeholder="What is your name?">
    <button id="button" @click="getData()"> {{ clickMsg }} </button>
    <div id="result">
      {{ resultText(this.click) }}
      <p> {{ this.ageMsg }} </p>
      <p> {{ this.genMsg }} </p>
      <p> {{ this.natMsg }} </p>
      <p> {{ this.armyMsg }} </p>
    </div>
    <div id="loader"></div>
  </div>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'

Vue.prototype.$http = axios

var ohNo = true
var linkName = ''
var ageLink = 'https://api.agify.io/?name='
var natLink = 'https://api.nationalize.io/?name='
var genLink = 'https://api.genderize.io/?name='

export default {
  data: () => ({
    ohNo: true,
    click: false,
    clickMsg: '',
    ageMsg: '',
    genMsg: '',
    natMsg: '',
    armyMsg: '',
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
      this.showResultOrNot(false)
      this.click = true //Knappen har blivit tryckt på
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

        this.ohNo = false //Inget error finns i nuläget
        // return ageResponse, natResponse, genResponse //returnera ålder, nationalitet och könstillhörighet

      } catch (error) { //om det blir ett error, skriv det i terminalen (console) på hemsidan
        this.ohNo = true
        console.log(error)
        console.log(ohNo)
      }

      this.showResultOrNot(true)
    },
    reload () {
      window.location.reload()
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
          this.ageMsg = 'Your age is ' + this.age
          this.genMsg = 'You are most likely a ' + this.gender
          this.natMsg = 'Your country ID is: ' + this.nationality + ' with a probablitiy of: ' + this.natProbability + '%'
          this.armyMsg = 'Your ' + this.linkName + ' army would contain ' + this.count + ' soldiers.'
          this.clickMsg = 'Refresh!'
        }
      }
      else { //om den inte blivit klickad på
        this.ageMsg = 'Press the button to predict!',
        this.genMsg = ' ',
        this.natMsg = ' ',
        this.armyMsg = ' ',
        this.clickMsg = 'Predict your age!'
      }
    },
    showResultOrNot (show) {
      console.log(show)
      if (show) {
        document.getElementById("result").style.display = 'block';
        document.getElementById("loader").style.display = 'none';
      }
      else {
        document.getElementById("result").style.display = 'none';
        document.getElementById("loader").style.display = 'block';
      }
    }
  }
}
</script>

<style>
/* Hur ska sidan se ut, designmässigt */
#div {
  width: 100vw;
  height: 100vh;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: large;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: blanchedalmond;
  color: black;
  background-image: url('C:/utveckling/agePredicter/website/src/assets/crowd.png');
  background-repeat: no-repeat;
  background-size: 100%;
  overflow: hidden;
  margin: -10px;
}

/* Hur ska knappen se ut */
#button {
  color: rgb(22, 21, 18);
  background-color: rgb(145, 134, 117);
  margin-left: 1%;
  margin-right: 5%;
  margin-top: 10%;
  margin-bottom: 5%;
  border: 5px double rgb(51, 48, 41);
}

/* Hur ska input-rutan se ut */
#input {
  color: rgb(22, 21, 18);
  background-color: rgb(145, 134, 117);
  border: 5px double rgb(51, 48, 41);
  margin-right: 1%;
  margin-left: 5%;
  margin-top: 5%;
  margin-bottom: 5%;
  outline: none;
}

/* Färg på placeholder (texten i input-rutan innan man skriver där) */
::placeholder {
  color: rgb(22, 21, 18);
}

/* Hur ska texten se ut */
#result {
  align-content: center;
  align-self: center;
  color: 22, 21, 18;
  background-color: rgb(145, 134, 117);
  margin-top: 5%;
  margin-left: 30%;
  margin-right: 30%;
  text-align: center;
  padding: 5px;
  border: 15px double rgb(51, 48, 41);
}

/* Hur ska titeln på sidan se ut? */
#title {
  color: rgb(51, 48, 41);
  background-color: transparent;
  width: 250px;
  height: 75px;
  font-size: 200%;
  border: none;
  margin-right: 85%;
}

#loader {
  border: 16px solid #f3f3f3;
  border-top: 16px solid rgb(22, 21, 18);
  border-radius: 50%;
  width: 100px;
  height: 100px;
  animation: spin 2s linear infinite;
  position: absolute;
  left: 50%;
  top: 70%;
  z-index: 1;
  margin: -76px 0 0 -76px;
  -webkit-animation: spin 2s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>