<template>
  <div id="hangeul">
    <div id="inputField">
      <h2>Mikä on nimesi koreaksi?</h2>
      <div class="inputOne">
        <input type="text" v-model="textConvert" />
      </div>
      <div class="inputTwo">
        <input type="submit" value="Lähetä" v-on:click="changeText" />
      </div>
      <div class="korName">
        {{ text }}
      </div>
    </div>
    <div class="component">
      <Hangeul msg="Annyeonghaseyo!" />
    </div>
  </div>
</template>

<script>
import Hangeul from "./components/Hangeul.vue";
import * as Hangul from "hangul-js";

export default {
  name: "hangeul",
  components: {
    Hangeul
  },
  data() {
    return {
      textConvert: "",
      text: "",
      alphabet: {
        a: "ㅏ",
        o: "ㅓ",
        å: "ㅓ",
        u: "ㅜ",
        e: "ㅔ",
        i: "ㅣ",
        y: "ㅣ",
        1: "ㅡ",
        0: "ㅇ",
        ja: "ㅑ",
        jo: "ㅕ",
        ju: "ㅠ",
        je: "ㅖ",
        jä: "ㅒ",
        jö: "ㅛ",
        va: "ㅘ",
        vo: "ㅝ",
        vu: "ㅢ",
        ve: "ㅞ",
        vi: "ㅟ",
        vä: "ㅙ",
        vö: "ㅚ",
        ä: "ㅐ",
        ö: "ㅚ",
        c: "ㅋ",
        f: "ㅍ",
        g: "ㄱ",
        n: "ㄴ",
        d: "ㄷ",
        r: "ㄹ",
        l: "ㄹ",
        m: "ㅁ",
        b: "ㅂ",
        s: "ㅅ",
        ng: "ㅇ",
        zh: "ㅈ",
        ch: "ㅊ",
        k: "ㅋ",
        t: "ㅌ",
        p: "ㅍ",
        h: "ㅎ",
        kk: "ㄲ",
        tt: "ㄸ",
        pp: "ㅃ",
        ss: "ㅆ"
      }
    };
  },
  methods: {
    stringSplit(string) {
        var arr = [],
          l,
          j = -1;

        for (var i = 0; i < string.length; i++) {
          var c = string.charAt(i);

          if (c == "l" && l == "l") {
            arr[++j] = c;
          } else
          if (c == "n" && l == "n") {
            arr[++j] = c;
          } else
          if (c == "m" && l == "m") {
            arr[++j] = c;      
          } else if (l == c) {
            arr[j] += c;
          } else if (c == "g" && l == "n") {
            arr[j] += c;
          } else if (c == "i" && l == "v") {
            arr[j] += c;
          } else if (c == "a" && l == "v") {
            arr[j] += c;
          } else if (c == "e" && l == "v") {
            arr[j] += c;
          } else if (c == "o" && l == "v") {
            arr[j] += c;
          } else if (c == "u" && l == "v") {
            arr[j] += c;
          } else if (c == "ö" && l == "v") {
            arr[j] += c;
          } else if (c == "ä" && l == "v") {
            arr[j] += c;
          } else if (c == "a" && l == "j") {
            arr[j] += c;
          } else if (c == "e" && l == "j") {
            arr[j] += c;
          } else if (c == "i" && l == "j") {
            arr[j] += c;
          } else if (c == "o" && l == "j") {
            arr[j] += c;
          } else if (c == "u" && l == "j") {
            arr[j] += c;
          } else if (c == "ä" && l == "j") {
            arr[j] += c;
          } else if (c == "ö" && l == "j") {
            arr[j] += c;
          } else {
            arr[++j] = c;
          }
          l = c;
        }
        console.log(arr)
        return arr;
    },
    modifyString(string) {

      function addtoVowel(match) {   
        if (match.length == 2) {
          return match[0] + "0" + match[1];  
        } else if (match.length == 3) { 
          return match[0] + "0" + match[1] + "0" + match[2]; 
        }
      }

      function addIfConsonant(match) {
        if (match.length == 2) {
          if (match[0] === match[1]) {
            return match;
          } else if (match[0] === "r" && match[1] === "j") {
            return match[0] + 1 + 0 + match[1];
          } else if (match[0] === "r" && match[1] === "v") {
            return match[0] + 1 + 0 + match[1];
          } else if (match[0] === "r" && match[1] === "k") {
            return match[0] + 1 + match[1];
          } else if (match[0] === "l" && match[1] === "j") {
            return match[0] + "l" + match[1];
          } else if (match[0] === "l" && match[1] === "v") {
            return match[0] + "l" + match[1];
          } else if (match[0] === "n" && match[1] === "g") {
            return match + 0;
          } else if (
            match[0] === "n" ||
            match[0] === "p" ||
            match[0] === "l" ||
            match[0] === "m" ||
            match[0] === 't'
          ) {
            return match;
          } else if (match === "ks") {
            return match;
          } else {
            return match[0] + 1 + match[1];
          }
        } else if (match.length === 3) {
          if (match[0] === 'r') {
            return (match[0] + 1 + match[1] + match[2])
          } else if (match[1] === 's') {
            return (match[0] + match[1] + 1 + match[2]) 
          } else {
            return match
          }
        }
      }

      function isVowel(string) {
        if ("aeiouäöy".indexOf(string[0]) != -1) {
          string = "0" + string
        }
        return string;
      }

      console.log(string)

      let newStr = string
        .replace(/\s/g, "")
        .replace(/[aeiouäöy]{2,}/gi, addtoVowel)
        .replace(/[^aeiouäöy]{2,}/gi, addIfConsonant)
        .replace(/j/g, "0j")
        .replace(/v/g, "0v")
        .replace(/[s]$/, "s1")
        

      console.log(newStr)

      return this.stringSplit(isVowel(newStr));
    },
    changeText() {
      let hangulArray = [];
      let textToHangul = this.modifyString(
        this.textConvert.trim().toLowerCase()
      );
      let arrayOfKeys = Object.keys(this.alphabet);

      for (let i = 0; i < textToHangul.length; i++) {
        for (let key of arrayOfKeys) {
          if (key === textToHangul[i]) {
            hangulArray.push(this.alphabet[key]);
          }
        }
      }

      this.text = Hangul.assemble(hangulArray);

      this.textConvert = "";
    }
  }
};
</script>

<style></style>
