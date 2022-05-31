<template>
  <div id="div">
  <!-- anropar funktionen reload() -->
    <button id="title" @click="reload()">
      AgePredicter
    </button>
    <br>
    <!-- Input, det som matas in sparas i variabeln "name" -->
    <input id="input" :value="this.name" @input="event => this.name = event.target.value"
      placeholder="What is your name?"> <!-- När inget matats in står detta -->

    <!-- Anropar funktionen getData() och har en text på sig som bestäms av clickMsg som beror av funktionen resultText() -->
    <button id="button" @click="getData()"> {{ clickMsg }} </button>
    <!-- Olika text beroende på funktionen resultText() -->
    <div id="result">
      {{ resultText(this.click) }}
      <!-- Varje variabel nedan beror på funktionen ovan -->
      <p> {{ this.ageMsg }} </p>
      <p> {{ this.genMsg }} </p>
      <p> {{ this.natMsg }} </p>
      <p> {{ this.armyMsg }} </p>
    </div>
    <!-- Beroende på getData() visas antingen resultatet (ovan) eller "laddar"-indikatorn (nedan) -->
    <div id="loader"></div>
  </div>
</template>

<script>
// Importera allt nödvändigt
import axios from 'axios'
import Vue from 'vue'

// Gör det möjligt att enkelt använda .get funktioner m.m.
Vue.prototype.$http = axios

// De variabler som behöver definieras
var linkName = ''
var ageLink = 'https://api.agify.io/?name='
var natLink = 'https://api.nationalize.io/?name='
var genLink = 'https://api.genderize.io/?name='
var countryLink = 'https://restcountries.com/v2/alpha/'
// var flagLink = 'https://countryflagsapi.com/png/'

export default {
  data: () => ({
    // Alla variabler som används i skriptet
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
    country: '',
    countryID: '',
    linkName: '',
    natProbability: 0,
    ageLink: 'https://api.agify.io/?name=',
    natLink: 'https://api.nationalize.io/?name=',
    genLink: 'https://api.genderize.io/?name=',
    countryLink: 'https://restcountries.com/v2/alpha/',
    // flagLink: 'https://countryflagsapi.com/png/',
    ageResponse: '',
    natResponse: '',
    genResponse: '',
    countryResponse: ''
  }),

  methods: {
    async getData () { //Anropar API:er och definierar samt tolkar svaren
      this.showResult(false) //"Laddar"-indikatorn visas här
      this.click = true //Knappen har blivit tryckt på
      try {
        linkName = this.name //lokal variabel till name från input
        let ageResponse = await this.$http.get( //API-anrop för ålder med länk och inmatat namn
          ageLink + linkName
        )
        let genResponse = await this.$http.get( //API-anrop för könstillhörighet
          genLink + linkName
        )
        let natResponse = await this.$http.get( //API-anrop för nationalitet
          natLink + linkName
        )

        //få ut rätt värde från API:ernas svar (response), istället för ett helt dictionary
        this.age = ageResponse['data']['age']
        this.count = Math.round(ageResponse['data']['count'])
        this.natProbability = Math.round((natResponse['data']['country'][0]['probability']) * 100)

        //Ändra från landets ID till landets namn och flagga
        this.countryID = natResponse['data']['country'][0]['country_id']
        let countryResponse = await this.$http.get( //API-anrop för landsnamn
          countryLink + this.countryID
        )

        this.country = countryResponse['data']['name']
        console.log(countryResponse)
        console.log(this.country)
        console.log(this.countryID)
        
        //byt ut "female" och "male" mot "woman" och "man"
        if (genResponse['data']['gender'] == 'female') { 
          this.gender = 'woman'
        }
        else if (genResponse['data']['gender'] == 'male') {
          this.gender = 'man'
        }

        this.ohNo = false //Inget error finns i nuläget

      } catch (error) { //om det blir ett error, skriv vad i terminalen (console) på hemsidan
        this.ohNo = true //det finns ett error
        console.log(error)
      }

      this.showResult(true) //Nu får resultatet visas och "laddar"-indikatorn göms
    },
    reload () {
      window.location.reload() //Laddar om sidan
    },
    resultText (click) { //Beroende på om knappen blivit intryckt, skriv ut text
      if (click) { //Om knappen använts (blivit klickad på)
        if (this.ohNo) { //Om det finns ett error
          this.ageMsg = 'Something went wrong, try another name!',
          this.genMsg = ' ',
          this.natMsg = ' ',
          this.armyMsg = ' ',
          this.clickMsg = 'Try again!'
        }
        else { //Om det inte finns ett error
          this.ageMsg = 'Your age is ' + this.age
          this.genMsg = 'You are most likely a ' + this.gender
          this.natMsg = 'Your are from: ' + this.country + ' with a probablitiy of: ' + this.natProbability + '%'
          this.armyMsg = 'Your ' + this.linkName + ' army would contain ' + this.count + ' soldiers.'
          this.clickMsg = 'Refresh!'
        }
      }
      else { //Om knappen inte använts
        this.ageMsg = 'Press the button to predict!',
        this.genMsg = ' ',
        this.natMsg = ' ',
        this.armyMsg = ' ',
        this.clickMsg = 'Predict your age!'
      }
    },
    showResult (show) { //Bestämmer om resultat eller en "laddar"-indikator ska visas
      if (show) { //Om show är true
        document.getElementById("result").style.display = 'block'; //Visa "result"
        document.getElementById("loader").style.display = 'none'; //Göm "loader"
      }
      else { //Om show är false
        document.getElementById("result").style.display = 'none'; //Göm "result"
        document.getElementById("loader").style.display = 'block'; //Visa "loader"
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

/* Hur ska input se ut */
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

/* Färg på placeholder (texten i input när den är tom) */
::placeholder {
  color: rgb(22, 21, 18);
}

/* Hur ska texten i result se ut */
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

/* Hur ska "laddar"-indikatorn se ut */
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
  display: none;
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