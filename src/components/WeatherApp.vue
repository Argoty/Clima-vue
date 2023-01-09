<template>
  <v-container>
    <v-form @submit.prevent="getWeather">
      <v-text-field
        v-model="location"
        placeholder="Ingresa una ubicación"
        color="brown darken-3"
        @input="error = false"
        clearable
      />
      <v-btn type="submit" color="brown darken-3" class="white--text"
        >Obtener clima</v-btn
      >
    </v-form>

    <SpinnerLoader v-if="loader" class="mt-7 mx-auto" />

    <v-container class="mt-3" v-if="weatherData.name">
      <v-layout row wrap>
        <v-flex xs12>
          <h2>Clima Actual de {{ weatherData.name }}</h2>
          <p class="font-weight-regular text-caption">{{ fechaActual }}</p>
          <v-card>
            <v-card-text>
              <p>
                <v-icon class="mr-2">mdi-weather-cloudy</v-icon>Descripción:
                <span class="font-weight-medium">{{
                  weatherData.weather[0].description
                }}</span>
              </p>
              <p>
                Temperatura:
                <span class="font-weight-medium">{{ weatherData.temp }}</span
                ><v-icon>mdi-temperature-celsius</v-icon>
              </p>
              <p>
                <v-icon>mdi-water-percent</v-icon>Humedad:
                <span class="font-weight-medium">{{
                  weatherData.main.humidity
                }}</span
                >%
              </p>
              <p>
                <v-icon>mdi-weather-windy</v-icon>
                Velocidad del viento:
                <span class="font-weight-medium">{{
                  weatherData.wind.speed
                }}</span>
                mph
              </p>

              <p>
                Visibilidad:
                <span class="font-weight-medium">{{
                  weatherData.visibility / 1000
                }}</span>
                Km
              </p>
            </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>

    <v-alert
      text
      prominent
      type="error"
      icon="mdi-cloud-alert"
      :value="error"
      class="mt-5"
    >
      La ubicación que pusiste no existe o no está registrada, por favor vuelve
      a intentarlo.
    </v-alert>
  </v-container>
</template>

<script>
import SpinnerLoader from "./SpinnerLoader.vue";

export default {
  name: "WeatherApp",
  components: {
    SpinnerLoader,
  },
  data() {
    return {
      location: "",
      weatherData: { wind: {}, weather: [{}], main: {} },
      loader: false,
      error: false,

      fechaActual: "",
    };
  },

  methods: {
    async getWeather() {
      const location = this.location.trim();
      if (location) {
        this.loader = true;

        try {
          const response = await fetch(
            `https://api.openweathermap.org/data/2.5/weather?q=${location}&appid=30cb47724866050bd0631cb61007fc7f`
          );
          let data = await response.json();

          this.loader = false;

          let fecha = new Date();
          let fechaFormateada = fecha.toLocaleDateString();
          let horaFormateada = fecha.toLocaleTimeString();

          this.fechaActual = fechaFormateada + " " + horaFormateada;

          data.temp = (data.main.temp - 273.15).toFixed(2);
          this.weatherData = { ...data };
        } catch (error) {
          this.loader = false;
          this.error = true;
          console.error(error);
        }
      }
    },


    async traducir(str) {
      let url = `https://translation.googleapis.com/language/translate/v2?key=${API_KEY}&q=${str}&target=es`;
      let response = await fetch(url);
      let data = await response.json();
      return data.data.translations[0].translatedText;
    },
  },
};
</script>