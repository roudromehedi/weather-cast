<template>
  <v-app class="bg-image">
    <main>
      <v-container>
        <div class="search-box">
          <v-text-field
            rounded
            label="Enter place name"
            placeholder="Ex. Berlin"
            v-model="query"
            outlined
            color="white"
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
        <weatherCard
          v-if="showcard"
          :img-url="imgUrl"
          :weather-data="weatherData"
        ></weatherCard
      ></v-container>
    </main>
  </v-app>
</template>

<script>
import weatherCard from "@/components/weather-card.vue";
export default {
  components: { weatherCard },
  name: "HomePage",
  data() {
    return {
      api_key: "7e0c6d3b1a4d4843068bc9a536e7dedf",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weatherData: {},
      imgUrl: "",
      showcard: false,
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
      this.showcard = true;
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
  background-image: url("../assets/cold.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.weather-cast-cold {
  background-image: url("../assets/cold.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.app.warm {
  background-image: url("../assets/warm.jpg");
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
.bg-image {
  background-image: url("../assets/cold.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed;
}
.theme--light.v-application {
  background-color: transparent;
}
</style>
