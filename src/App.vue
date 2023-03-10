<template>
  <div
    id="app"
    :class="
      typeof weather.main !== 'undefined' && weather.main.temp > 16
        ? 'warm'
        : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ normalizeDate() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { Weather } from "./types/weather";

export default defineComponent({
  name: "App",
  components: {},
  data() {
    return {
      api_key: "38d057bd87e726b8b5809fc8240e076a",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {} as Weather,
    };
  },
  methods: {
    fetchWeather(event: { key: string }) {
      if (event.key === "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then((res) => {
            this.weather = res;
          });
      }
    },
    normalizeDate() {
      return new Date().toLocaleString("en", {
        dateStyle: "long",
      });
    },
  },
});
</script>

<style lang="scss">
main {
  max-width: 540px;
  margin: 0 auto;
  min-height: 100vh;
  padding: 25px;
}

.search-box {
  margin-bottom: 30px;

  .search-bar {
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    border: none;
    outline: none;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;

    &:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0px 16px 0px;
    }
  }
}

.location-box {
  .location {
    font-size: 32px;
    font-weight: 500;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .date {
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
  }
}

.weather-box {
  .temp {
    display: inline-block;
    padding: 10px 25px;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather {
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
}
</style>
