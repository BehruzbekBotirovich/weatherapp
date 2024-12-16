<template>
  <div class="wrapper">


    <h1>Weather app</h1>
    <p>Know the weather in <b>{{ city == "" ? "city" : city }}</b></p>
    <input type="text" v-model="city" placeholder="Enter your city" @keyup.enter="getweat()">
    <button v-show="city != ''" @click="getweat()">Search</button>
    <p id="error">{{ error }}</p>

      <div id="infobar">

        <p v-show="info != null">The temperature is: <span>{{ showtemp }}°C</span> </p>
        <p v-show="info != null">Feels like:  <span>{{ showfeel }}°C</span></p>
        <p v-show="info != null">Weather is: <span>{{ weatheris }}</span> </p>
        <p v-show="info != null">Humidity:  <span>{{  humidityshow}}%</span></p>

      </div>  


    
  </div>
</template>

<script>
import { computed } from 'vue';

import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      error: '',
      info: null,  
    
    };
  },
  computed: { 
    showtemp() {
      return this.info ? this.info.main.temp : '';
    },
    showfeel() {
      return this.info ? this.info.main.feels_like : '';
    },
    weatheris() {
      return this.info ? this.info.weather[0].main : '';
    },
    humidityshow(){
      return this.info ? this.info.main.humidity : '';
    }
  
  },
  methods: {
    getweat() {
      if (this.city.trim().length < 2) {
        this.error = 'Input the real city';
        return;
      }
      this.error = '';
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=b85f93da422aeb2e4f550a567bf45977`)
        .then(res => (this.info = res.data))
        .catch(error => {
          this.error = 'Error fetching data';
          console.error(error);
        });
    }
  }
};
</script>

<style scoped>
.wrapper {
  width: 800px;
  height: 400px;
  border-radius: 40px;
  padding: 20px;
  background-color: #0f1a39;
  color: antiquewhite;
  text-align: center;
  font-size: 25px;
}


#infobar p span{
color: gold;
}
.wrapper h1 {
  margin-top: 30px;
}

.wrapper input {
  margin-top: 30px;
  font-size: 14xp;
  background-color: transparent;
  border: 1px solid white;
  color: azure;
  padding: 5px 18px;
  transition: ease 100ms;
}

.wrapper input:focus {}

#error {
  color: red;
}

.wrapper button {
  background-color: #7804aa;
  /* Green */
  border: none;
  color: white;
  padding: 10px 30px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 10px;
}

#icon img {}
</style>

