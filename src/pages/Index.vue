<template>
  <q-page class="flex column justify-center">
    <div class="col q-pt-lg q-px-md row justify-center  text-center">
      <q-input
        @keyup.enter="getWeatherInput"
        class="searchField fixed-top-center"
        rounded
        filled
        bottom-slots
        v-model="search"
        dark
        color="lime-11"
        label="Введите локацию..."
      >
        <template v-slot:prepend>
          <q-icon name="place" />
        </template>
        <template v-slot:append>
          <q-icon
            @click="getWeatherInput"
            name="search"
            class="cursor-pointer"
          />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center  q-pd-lg  ">
        <div class="text-h4 text-weight-light">
          {{ weatherData.location.name }}
        </div>

        <div class="row justify-center">
          <q-card class="my-card">
            <div class="text-h1 text-weight-thin q-my-lg relative-position">
              <span class="degree_number">
                {{ Math.round(weatherData.current.temp_c) }}</span
              >
              <span
                class="text-h3 text-weight-thin relative-position degree_circle"
              >
                &deg;c
              </span>
            </div>
            <div>
              {{ weatherData.location.localtime }}
            </div>
            <q-list>
              <q-item clickable>
                <q-item-section avatar>
                  <img
                    :src="`${weatherData.current.condition.icon}`"
                    alt="weather-img"
                  />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    <div class="text-h6 text-weight-light">
                      {{ weatherData.current.condition.text }}
                    </div>
                  </q-item-label>
                </q-item-section>
              </q-item>

              <q-item clickable>
                <q-item-section avatar>
                  <q-icon
                    style="font-size: 2rem; margin-left:32%"
                    color="primary"
                    name="water"
                  />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    <div class="text-h5 text-weight-medium">
                      {{ weatherData.current.humidity }}%
                    </div>
                  </q-item-label>
                  <q-item-label caption>humidity</q-item-label>
                </q-item-section>
              </q-item>

              <q-item clickable>
                <q-item-section avatar>
                  <q-icon
                    style="font-size: 2rem; margin-left:32%"
                    color="primary"
                    name="speed"
                  />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    <div class="text-h5 text-weight-medium">
                      {{ Math.round(weatherData.current.wind_kph) }} km/h
                    </div>
                  </q-item-label>
                  <q-item-label caption>wind</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-card>
        </div>
      </div>
    </template>

    <template v-else>
      <div class="col column text-center text-white q-pa-lg">
        <div class="col text-h2 text-weight-thin">
          Vuether <br />
          <span class="text-h6 text-weight-thin ">
            Лучшая погода только у нас))</span
          >
          <br />
          <q-btn
            @click="getLocation"
            rounded
            color="secondary"
            label="Мое местоположение"
            icon="place"
          />
        </div>

        <small>All right reserved by OptimaBank, Kyrgyzstan</small>
      </div>
    </template>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: "https://api.weatherapi.com/v1/current.json?",
      apiKey: "key=6d64fdaa7883482db6d161828211706"
    };
  },

  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(position => {
        console.log(position);
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
        this.getWeatherByCoords();
      });
    },
    getWeatherByCoords() {
      this.$axios(
        `${this.apiUrl}${this.apiKey}&q=${this.lat},${this.lon}&aqi=yes`
      ).then(response => {
        this.weatherData = response.data;
      });
    },

    getWeatherInput() {
      this.$axios(`${this.apiUrl}${this.apiKey}&q=${this.search}&aqi=yes`)
        .then(response => {
          this.weatherData = response.data;
        })
        .catch(err => alert(err));
    }
  }
};
</script>

<style lang="sass">

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600&display=swap')

.q-page
     background-image: linear-gradient( rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.25) ), url("https://images.unsplash.com/photo-1543500422-bf591449002c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1034&q=80")
     background-size: cover



.searchField
    width: 70%

.degree_circle
    top: -40px
    color: #fff

.degree_number
    color: #fff

.lozung
    top: -40px


div img
    width: 50px


.my-card
  width: 100%
  max-width: 300px
  background: linear-gradient(to left, #eecda3, #ef629f)
  border-radius: 10%
</style>
