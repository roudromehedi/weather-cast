<template>
  <div>
    <main class="app" d-flex justify-center>
      <v-container
        ><div class="search-box">
          <v-text-field
            rounded
            label="Enter place name"
            placeholder="Ex. Berlin"
            v-model="query"
            outlined
            color="red"
            shaped
            @keypress="fetchWeather"
          />
        </div>
        <div class="div">
          <img
            v-if="!weatherData.name"
            height="200"
            class=""
            src="../assets/weather-cast-logo.png"
          />
        </div>

        <div class="weather-wrap" v-if="typeof weatherData.main != 'undefined'">
          <div class="location-box">
            <div class="location">
              {{ weatherData.name }}, {{ weatherData.sys.country }}
            </div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">
              {{ Math.round(weatherData.main.temp) }}°c
              <span><img :src="imgUrl" /></span>
            </div>
            <div class="weather">{{ weatherData.weather[0].main }}</div>
          </div>
        </div></v-container
      >
    </main>
  </div>
</template>

<script>
export default {
  components: {},
  name: "HomePage",
  data() {
    return {
      api_key: "7e0c6d3b1a4d4843068bc9a536e7dedf",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weatherData: {},
      imgUrl: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },

    setResults(results) {
      this.weatherData = results;
      this.getIcon();
    },
    getIcon() {
      const iconCode = this.weatherData.weather[0].icon;
      const iconUrl = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;

      return (this.imgUrl = iconUrl);
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style lang="scss">
body {
  font-family: "montserrat", sans-serif;
}
.app {
  background-image: url("../assets/cold.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.app.warm {
  background-image: url("../assets/warm.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.div {
  height: 100%;
  width: 100%;
  margin: auto;
  margin-bottom: auto;
}

.div img {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
.theme--light.v-text-field--outlined:not(.v-input--is-focused):not(
    .v-input--has-state
  )
  > .v-input__control
  > .v-input__slot
  fieldset {
  color: rgba(255, 255, 255, 0.38);
}
</style>
