<template>
  <div id="app">
      <form id="_main-form" action="">
        <GeneralInfo v-if="slide=1" key="slide1" ></GeneralInfo>
        <SignUp v-else-if="slide=2" key="slide2"></SignUp>
        <div id="_buttons-container">
          <button id="_prev" v-on:click="prevSlide()">Prev</button>
          <button id="_next" v-on:click="nextSlide()">Next</button>
        </div>
      </form>

  </div>
</template>

<script>
import GeneralInfo from '@/components/GeneralInfo.vue'
import SignUp from '@/components/SignUp.vue'

export default {
  name: 'home',
  components: {
    GeneralInfo,
    SignUp
  },
  data () {
    return {
      isHidden: false,
      slide: 1,
      slides: [1, 2]
    }
  },
  methods: {
    nextSlide: function () {
      this.slide++
      console.log('Next', this.slide)
    },
    prevSlide: function () {
      if (this.slide === 1) {
        return this.slide
      } else {
        this.slide--
      }
      console.log('Prev', this.slide)
    }
  }
}
</script>
<style lang="scss">
  $themeColor: teal;
  .warningClass {
    outline: 2px solid red !important;
    border: 1px solid transparent;
  }
  .error-msg {
    color: red;
    font-size: 12px;
    font-style: italic;
    visibility: hidden;
  }
  .show {
    visibility: visible;
  }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
form {
  max-width: 50%;
  min-width: 320px;
  border: 1px solid;
  margin: 0 auto;
  padding: 40px 20px;
  input[type="submit"] {
    margin-top: 20px;
    color: white;
    border-radius: 4px;
    background-color: $themeColor;
    cursor: pointer;
  }
  >div {
    text-align: left;
  }
  input, select {
    background-image: none;
    margin: 0;
    width: 100%;
    box-sizing: border-box;
    padding: 10px;
    border: 1px solid;
    color: $themeColor;
    font-size: 16px;
  }
  input::placeholder {
    color: $themeColor;
    font-size: 16px;
  }
  label {
    font-size: 12px;
  }
  .checkbox-wrapper {
    display: flex;
    flex-direction: column;
    align-items: baseline;
    input[type=checkbox] {
      display: none;
    }
    input[type=checkbox] ~ label::before {
      content: '\2713';
      display: inline-block;
      text-align: center;
      font-size: 16px;
      color: white;
      line-height: 20px;
      width: 20px;
      height: 20px;
      border: 1px inset silver;
      border-radius: 4px;
      cursor: pointer;
    }
    input[type=checkbox]:checked ~ label::before {
      color: $themeColor;
    }
  }
  #_buttons-container {
    display: flex;
    margin-top: 40px;
    flex-direction: row;
    justify-content: space-between;
    #_prev, #_next {
      border: none;
      padding: 10px 20px;
      border-radius: 4px;
      color: white;
      background-color: $themeColor;
      cursor: pointer;
    }
  }
}
</style>
