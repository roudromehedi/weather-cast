<template>
  <v-card class="blurred-card px-10">
    <div class="px-10 py-10">
      <div class="location-box">
        <div class="weather-cast-location">
          {{ weatherData.name }}, {{ weatherData.sys.country }}
        </div>
        <div class="weather-cast-location-date">{{ formattedDate }}</div>
      </div>

      <v-layout class="parent">
        <v-flex>
          <div class="weather-cast-temperature ms-auto">
            {{ Math.round(weatherData.main.temp) }}°c
          </div>
          <div>Feels like:{{ Math.round(weatherData.main.feels_like) }}°c</div>
        </v-flex>
        <v-flex class="weather-cast-type me-auto ml-2">
          <img class="" :src="imgUrl" />
          <div class="">{{ weatherData.weather[0].description }}</div>
        </v-flex>
      </v-layout>
    </div>
    <div>Max:</div>
  </v-card>
</template>

<script>
import moment from "moment";
export default {
  name: "weatherCard",
  data() {
    return {
      message: "Hello World!",
    };
  },
  props: {
    weatherData: {
      type: Object,
      required: true,
    },
    imgUrl: { type: String, required: true },
  },
  computed: {
    formattedDate() {
      return moment().format("MMMM Do YYYY, h:mm a");
    },
  },
};
</script>
<style lang="scss" scoped>
.blurred-card {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(1.9px);
  -webkit-backdrop-filter: blur(1.9px);
  border: 1px solid rgba(255, 255, 255, 0.5);
}
.parent {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 1fr;
  grid-column-gap: px;
  grid-row-gap: 0px;
  color: white;
}

.weather-cast-temperature {
  color: white;
  font-size: 100px;
}
.weather-cast-type {
  color: white;
}
.weather-cast-location {
  color: white;
  font-size: 50px;
}
.weather-cast-location-date {
  color: white;
  font-size: 15px;
}
</style>
