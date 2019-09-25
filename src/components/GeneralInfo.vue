<template>
  <div id="_general-info">
    <div class="input-block">
      <label for="firstname">First Name</label>
      <input id="firstname" ref="firstname" type="text" name="firstname" placeholder="First Name"
             v-model="firstNameVal"
             @focus="isFocused('firstname')"
             @blur="isBlurred"
             :class="{warningClass: !firstNameVal || this.isCorrect.firstname.wasFocused}" required>
            <span class="error-msg" :class="{show: !firstNameVal || this.isCorrect.firstname.wasFocused}">{{isCorrect
              .firstname.error}}</span>
    </div>

    <div class="input-block">
      <label for="lastname">Last Name</label>
      <input id="lastname" ref="lastname" type="text" name="lastname" placeholder="Last Name"
             v-model="lastNameVal"
             @focus="isFocused('lastname')"
             @blur="isBlurred"
             :class="{warningClass: !lastNameVal || isCorrect.lastname.wasFocused}" required>
      <span class="error-msg" :class="{show: !lastNameVal || isCorrect.lastname.wasFocused}">{{isCorrect.lastname
        .error}}</span>
    </div>

    <div class="input-block">
      <label for="selectCountry">Choose your country</label>
      <select id="selectCountry"
              name="countryVal"
              v-model="isCorrect.select.countryObj"
              @blur="comparePostCodeWithCountry">
        <option value="Choose your country" disabled selected>Choose your country</option>
        <option v-for="(el,index) in countries"
                :value="el"
                :key="index">{{el.Country}}</option>
      </select>
    </div>

    <div class="input-block">
      <label for="city">City</label>
      <input id="city" type="text" name="city" placeholder="City" v-model="isCorrect.city.name"
             @focus="isFocusedCity"
             :class="{warningClass: isCorrect.city.wasFocused }"
             @input="isString">
      <span class="error-msg" :class="{show: isCorrect.city.wasFocused }">{{isCorrect.city
        .error}}</span>
    </div>

    <div class="input-block">
      <label for="address">Address</label>
      <input id="address" name="address" type="text"
             v-model="isCorrect.address.value"
             @blur="isFirstCharNumber"
             placeholder="Address"
             :class="{warningClass: !isCorrect.address.isValid && isCorrect.address.wasFocused}">
      <span class="error-msg"
            :class="{show: !isCorrect.address.isValid && isCorrect.address.wasFocused}">{{isCorrect.address
        .error}}</span>
    </div>

    <div class="input-block">
      <label for="postalCode">Postal code</label>
      <input id="postalCode" type="text" placeholder="Postal code"
             v-model="isCorrect.postalCode.value"
             @blur="comparePostCodeWithCountry"
             :class="{warningClass: isCorrect.postalCode.isValid === false && !isCorrect.postalCode.wasFocused}">
      <span class="error-msg"
            :class="{show: isCorrect.postalCode.isValid === false && !isCorrect.postalCode.wasFocused}">
        {{isCorrect.postalCode.error}}</span>
    </div>

    <div class="checkbox-wrapper">
      <input id="_saveInfo-checkbox"
             type="checkbox" v-model="checked"
             @click="changeState">
      <label for="_saveInfo-checkbox"></label>
    </div>

    <div id="savedAddress-block"
    v-show="state">
      <div class="input-block">
        <label for="selectCountryAddress">Choose your country</label>
        <select id="selectCountryAddress" name="">
          <option value="Choose your country" disabled selected>Choose your country</option>
          <option v-for="(el,index) in countries"
                  :value="el"
                  :key="index">{{el.Country}}</option>
        </select>
      </div>

      <div class="input-block">
        <label for="cityAddress">City</label>
        <input id="cityAddress" type="text" name="city" placeholder="City" v-model="isCorrect.city.name">
      </div>

      <div class="input-block">
        <label for="addressAddress">Address</label>
        <input id="addressAddress" name="address" type="text"
               v-model="isCorrect.address.value"
               placeholder="Address">
      </div>

      <div class="input-block">
        <label for="postalCodeAddress">Postal code</label>
        <input id="postalCodeAddress" type="text" placeholder="Postal code"
               v-model="isCorrect.postalCode.value">
      </div>

    </div>

    <input type="submit" value="Submit"
           @click="submitForm">
  </div>
</template>

<script>
export default {
  name: 'GeneralInfo',
  data () {
    return {
      checked: true,
      submitted: false,
      firstNameVal: null,
      lastNameVal: null,
      warningClass: 'warningClass',
      countries: [],
      isCorrect: {
        firstname: {
          value: this.firstNameVal,
          error: 'Fill in your name, please',
          wasFocused: false
        },
        lastname: {
          value: this.lastNameVal,
          error: 'Fill in your surname, please',
          wasFocused: false
        },
        city: {
          name: '',
          error: 'Enter valid city name, please',
          wasFocused: false
        },
        address: {
          value: '',
          error: 'Fill in your address, please',
          isValid: false,
          wasFocused: false
        },
        select: {
          countryObj: 'Choose your country'
        },
        postalCode: {
          value: '',
          isValid: true,
          wasFocused: false,
          error: 'Postal code doesn\'t match for country'
        }
      },
      state: true
    }
  },
  mounted: function () {
    this.axios
      .get('https://gist.githubusercontent.com/jamesbar2/1c677c22df8f21e869cca7e439fc3f5b/raw/21662445653ac861f8ab81caa8cfaee3185aed15/postal-codes.json')
      .then(response => (this.countries = response.data))
  },
  methods: {
    changeState: function () {
      this.state = !this.state
    },
    isFocused: function (e) {
      this.isCorrect[e].wasFocused = false
    },
    isBlurred: function () {
      let fName = this.$refs.firstname.value
      let lName = this.$refs.lastname.value

      if (fName === '' && this.isCorrect.firstname.wasFocused) {
        this.isCorrect.firstname.wasFocused = true
      }
      if (lName === '' && this.isCorrect.lastname.wasFocused) {
        this.isCorrect.lastname.wasFocused = true
      }
    },
    isString: function () {
      if (/^[a-zA-Z]+(?:[\s-][a-zA-Z]+)*$/.test(this.isCorrect.city.name)) {
        this.isCorrect.city.wasFocused = false
      } else {
        this.isCorrect.city.wasFocused = true
      }
    },
    isFocusedCity: function () {
      this.isCorrect.city.wasFocused = false
    },
    isFirstCharNumber: function () {
      let addressVal = this.isCorrect.address.value
      let firstChar = addressVal.charAt(0).trim()
      if (!Number(firstChar)) {
        this.isCorrect.address.isValid = false
        this.isCorrect.address.wasFocused = true
      } else {
        this.isCorrect.isValid = true
        this.isCorrect.address.wasFocused = false
      }
    },
    comparePostCodeWithCountry: function () {
      const regex = new RegExp(this.isCorrect.select.countryObj.Regex)
      const inputedVal = this.isCorrect.postalCode.value
      if (inputedVal === '') {
        this.isCorrect.postalCode.isValid = true
      } else if (regex.test(inputedVal)) {
        this.isCorrect.postalCode.isValid = true
        this.isCorrect.postalCode.wasFocused = false
        console.log('Yeah')
      } else {
        this.isCorrect.postalCode.isValid = false
        console.log('No-no')
      }
    },
    submitForm: function () {
      this.isFocused('firstname')
      this.isFocused('lastname')
      this.isBlurred()
      this.isString()
      this.isFocusedCity()
      this.isFirstCharNumber()
      this.comparePostCodeWithCountry()
    }
  },
  computed: {
  }

}
</script>

<style lang="scss" scoped>
  #_general-info {

  }
</style>
