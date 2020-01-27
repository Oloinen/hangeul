<template>
  <div id="app">
    <div id="inputField">
      <h2>Mikä on nimesi koreaksi?</h2>
      <br />
      <input type="text" v-model="textConvert" />
      <br />
      <input type="submit" value="Submit" v-on:click="changeText" />
      <br />
      <br />
      {{ text }}
    </div>
    <Hangeul msg="Annyeonghaseyo!" />
  </div>
</template>

<script>
import Hangeul from './components/Hangeul.vue'
import * as Hangul from 'hangul-js';

export default {
  name: 'app',
  components: {
    Hangeul
  },
  data() {
    return {
    textConvert: '',
    text: '',
    alphabet: {
      'a': "ㅏ", 'ja': "ㅑ", 'o':'ㅓ', 'jo':'ㅕ', 3:'ㅗ', 4:'ㅛ', 'u':'ㅜ', 'ju':'ㅠ', 1: 'ㅡ', 'i':'ㅣ', 0:'ㅇ', 'e':'ㅔ', 'je':'ㅖ', 'ö':'ㅚ', 'vu':'ㅝ', 'va':'ㅘ', 'vo':'ㅝ', 'vi':'ㅟ', 'ä':'ㅐ', 've':'ㅞ',
      'g':'ㄱ', 'n':'ㄴ','d':'ㄷ','r':'ㄹ', 'l':'ㄹ', 'm':'ㅁ', 'b':'ㅂ','s':'ㅅ', 'ng':'ㅇ', 'zh':'ㅈ', 'ch':'ㅊ', 
      'k':'ㅋ', 't':'ㅌ', 'p':'ㅍ', 'h':'ㅎ',
      'kk':'ㄲ', 'tt':'ㄸ', 'pp':'ㅃ', 'ss':'ㅆ'
                }
            }
  },
  methods: {
      consonantTampering(string) {
        const consReg = /([^aeiouöä])\1/g

        /*let nep = string.match(consReg).forEach(item => {
          if (item == "kk" || item == "tt" || item == "pp" || item == "ss") {
            return ("0")
          }
        })*/
        console.log(string)
        console.log(string.match(consReg))
        //console.log(nep)

        return string
      },

      modifyString(string) {
          const doubleVowelReg = /[aeiouäö]{2}/ig

          console.log(string)

          function addtoVowel(match) {
            return ( match[0] + '0' + match[1])
          }

          function isVowel(array) {
            if ("aeiouäö".indexOf(array[0]) != -1) {
              array = "0" + array
            }
            return array
          }
          let newArray = string.replace(doubleVowelReg, addtoVowel);

          return this.consonantTampering(isVowel(newArray))
       },
      changeText() {
      let hangulArray = []
      let textToHangul = this.modifyString(this.textConvert.trim().toLowerCase()).split('').map(item => item);
      let arrayOfKeys = Object.keys(this.alphabet);

        for (let i = 0; i < textToHangul.length; i++) {
          for (let key of arrayOfKeys) {
            if (key === textToHangul[i]) {
            hangulArray.push(this.alphabet[key])
            }
          }
            }
          console.log(hangulArray)
          console.log(Hangul.assemble(hangulArray))
          this.text = Hangul.assemble(hangulArray);

          this.textConvert = ''
        }

  }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
