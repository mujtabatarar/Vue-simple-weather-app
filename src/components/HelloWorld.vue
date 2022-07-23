<template>
  <div>
    <input
      class="inputField"
      type="text"
      placeholder="Enter City"
      v-model="city"
      @keypress="fetchApi"
    />
    <h1 class="cityHeading">{{ city }}</h1>
    <h2 class="dateClass">{{ date }}</h2>
    <div class="blurBox">
      <p class="tempText">{{ temperature }}</p>
    </div>

    <p class="overview">{{ overview }}</p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  data() {
    return {
      search: "lahore",
      city: "lahore",
      date: "",
      temperature: "",
      overview: "",
      sunrise: "",
      sunset: "",
      lat: "",
      lon: "",
    };
  },

  async mounted() {},

  methods: {
    async fetchApi(e) {
      console.warn("infetchApi");
      if (e.key == "Enter") {
        console.log("enterPressed");
        const locationResults = await axios.get(
          `http://api.openweathermap.org/geo/1.0/direct?q=${this.city}&limit=5&appid=9568693bb5d99cd9e7a11079bd670592`
        );

        this.lat = locationResults.data[0].lat;
        this.lon = locationResults.data[0].lon;

        console.warn(locationResults.data[0].lat);
        const weatherResutls = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.lon}&appid=9568693bb5d99cd9e7a11079bd670592`
        );
        console.warn(weatherResutls.data);
        this.city = weatherResutls.data.name;
        let today = new Date();
        this.date = today.toDateString();
        this.temperature = parseInt(weatherResutls.data.main.temp);
        this.temperature = this.temperature - 274.15;
        this.temperature = this.temperature.toPrecision(3);
        this.temperature = this.temperature + "c";
        this.overview = weatherResutls.data.weather[0].main;
        console.warn("oveeeer" + this.overview);
        this.sunrise = weatherResutls.data.sys.sunrise;
        this.sunset = weatherResutls.data.sys.sunset;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.inputField {
  font-size: 18pt;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  border-radius: 5px 50px 5px;
  border: 2px solid #609;
  text-align: center;
  padding: 20px;
  width: 60vw;
  height: 15px;
}
.cityHeading {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  color: azure;
  font-size: 32pt;
}
.tempText {
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  color: chartreuse;
  font-size: 52pt;
}
.blurBox {
  -webkit-backdrop-filter: blur(5px);
  backdrop-filter: blur(5px);
  background: rgba(239, 233, 233, 0.043);
  border-radius: 20px;
  
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 200px;
  max-height: 250px;
  min-width: 150px;
  max-width: 250px;
margin: 0 auto;
  
}
.dateClass {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  color: aliceblue;
}
.overview {
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
  color: aliceblue;
  font-size: 32px;
  font-style: italic;
}
</style>
