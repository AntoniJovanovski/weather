<template>
  <div class="main-container">
        <div>
          <the-header 
          @input="input" 
          @fav-city="favouriteFunction"
          ></the-header>

          <main-city-info 
          :city="city" 
          :newcity="passCityName"
          @weather-details="passDetails"
          @add-main-fav="addToFav"></main-city-info>

          <hourly-info 
          :city="city" 
          :newcity="passCityName"
          ></hourly-info>

          <weather-details 
          :details="weatherDetails"
          ></weather-details>
        </div>

      <favourite-cities 
      :favourite="favouriteCity" 
      :fav="addFav"
      @view-city="passNameFucntion"
      @delete="deleteFromFav"></favourite-cities>
  </div>
</template>

<script>

import TheHeader from './components/TheHeader.vue'
import MainCityInfo from './components/MainCityInfo.vue'
import HourlyInfo from './components/HourlyInfo.vue'
import WeatherDetails from './components/WeatherDetails.vue';
import FavouriteCities from './components/FavouriteCities.vue';


export default {
  components: {
    TheHeader,
    MainCityInfo,
    HourlyInfo,
    WeatherDetails,
    FavouriteCities
  },
  data() {
    return {
      city: '',
      favouriteCity: '',
      weatherDetails: '',
      passCityName: '',
      addFav: '',
    };
  },

  methods: {
    passNameFucntion(name) {
      this.passCityName = name
    },
    input(value) {
      this.city = value
    },
    favouriteFunction(val) {
      this.favouriteCity = val
    },
    passDetails(weather) {
      this.weatherDetails = weather
    },
    addToFav(name) {
      this.addFav = name
    },
    deleteFromFav() {
      this.addFav = ''
    },
  },
};
</script>
<style>
button:hover {
  cursor: pointer;
}
button:active {
  scale: .9;
}
body {
  font-family: 'Roboto', sans-serif;
  background: url('https://images.unsplash.com/photo-1496450681664-3df85efbd29f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1170&q=80');
  background-size: cover;
  margin: 0;
  overflow-x: hidden;
  width: 100vw;
}
li {
  list-style: none;
}
</style>
<style scoped>
.main-container {
  width: 100vw;
  display: flex;
  flex-direction: row;
  transition: 1s;
  justify-content: center;
}
h3 {
  text-align: center;
}
@media only screen and (max-width: 700px) {
  .main-container {
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: 1s;
    justify-content: center;
  }
}
</style>


