<template>
  <v-app>
    <v-app-bar
      app
      color="brown darken-3"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Climaexact Logo"
          class="shrink mr-2"
          contain
          src="./assets/weatherIcon.png"
          transition="scale-transition"
          width="60"
        />

        <h1>Climexact...</h1>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://openweathermap.org/"
        target="_blank"
        text
      >
        <span class="mr-2">OpenWeatherMap</span>
        <v-icon>mdi-sun-wireless</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <WeatherApp />
    </v-main>

    <Footer> </Footer>
  </v-app>
</template>

<script>
import WeatherApp from './components/WeatherApp';
import Footer from './components/Footer';

export default {
  name: 'App',

  components: {
    WeatherApp,
    Footer
  },

  data: () => ({
    //
  }),
};
</script>
