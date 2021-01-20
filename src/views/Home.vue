<template>
  <div class="home">
    <div class="heading">
      <h1>{{ cityName }}</h1>
      <div>
        {{ currentDay }}
      </div>
      <div>{{ weatherDescription }}</div>
    </div>
    <!-- End of Heading Section -->

    <div class="contents">
      <div class="left">
        <img :src="`${imageUrl}/${weatherIcon}@2x.png`" alt="" />
        <span class="temprature">{{ temprature }} </span
        ><span class="top">°C</span>
      </div>
      <div class="right">
        <div>Precipitation: {{ precipitation }} %</div>
        <div>Humidity: {{ humidity }} %</div>
        <div>Wind: {{ windSpeed }} km/h</div>

        <ul>
          <li>Temprature</li>
          <li>Rainfall</li>
          <li>Wind</li>
        </ul>
      </div>
    </div>
    <!-- End of Contents Section -->

    <div class="days-list">
      <div
        class="day-card"
        v-for="(day, i) of responseData"
        :key="day.dt"
        @click.prevent="selectDay(i)"
      >
        <h1>{{ getCurrentDay(i) }}</h1>

        <div>
          <img :src="`${imageUrl}/${day.weather[0].icon}@2x.png`" />
        </div>
        <span>{{ Math.round(day.temp.min - 273.15) }}°</span>
        <span>{{ Math.round(day.temp.max - 273.15) }}°</span>
      </div>
      <!-- End of Days list Section -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  components: {},
  data() {
    return {
      apiKey: process.env.VUE_APP_API_KEY,
      baseUrl: process.env.VUE_APP_BASE_URL,
      imageUrl: process.env.VUE_APP_IMAGE_GALARY_URL,
      responseData: [],
      selectedDayData: {},
      cityName: null,
      currentDay: null,
      weatherDescription: null,
      weatherIcon: null,
      temprature: null,
      precipitation: 0,
      humidity: 0,
      windSpeed: 0
    };
  },

  async created() {
    let position = await this.$getLocation();

    let res = await this.fetchWeatherData(
      this.baseUrl,
      position.lat,
      position.lng,
      this.apiKey
    );

    this.responseData = res.data.daily;
    this.selectedDayData = this.responseData[0];
    this.cityName = res.data.timezone;
    this.currentDay = this.getCurrentDay(0) + "Day";
    this.setWeatherData();
  },

  methods: {
    async fetchWeatherData(url, lat, lon, api_key) {
      try {
        let res = await axios.get(
          `${url}?lat=${lat}&lon=${lon}&appid=${api_key}`
        );
        return res;
      } catch (error) {
        console.log(error);
        return;
      }
    },
    setWeatherData() {
      this.weatherDescription = this.selectedDayData.weather[0].description;
      this.weatherIcon = this.selectedDayData.weather[0].icon;
      this.temprature = Math.round(this.selectedDayData.temp.day - 273.15);
      this.humidity = this.selectedDayData.humidity;
      this.windSpeed = this.selectedDayData.wind_speed;
      this.precipitation = this.selectedDayData.rain || 0;
    },
    selectDay(index) {
      this.selectedDayData = this.responseData[index];
      this.currentDay = this.getCurrentDay(index) + "Day";
      this.setWeatherData();
    },
    getCurrentDay(index) {
      let k = (new Date().getDay() + index) % 7;
      let day;
      switch (k) {
        case 0:
          day = "Sun";
          break;
        case 1:
          day = "Mon";
          break;
        case 2:
          day = "Tue";
          break;
        case 3:
          day = "Wed";
          break;
        case 4:
          day = "Thu";
          break;
        case 5:
          day = "Fri";
          break;
        default:
          day = "Sat";
      }
      return day;
    }
  },

  computed: {}
};
</script>
<style lang="scss" scoped>
* {
  //border: 1px solid red;
}

.home {
  margin: 0;
  padding: 5% 10%;
  color: grey;

  .heading {
    h1 {
      margin: 0;
    }
  }
  .contents {
    display: flex;
    justify-content: space-between;
    align-items: center;

    .left {
      display: flex;
      align-items: flex-start;

      .temprature {
        align-self: center;
        font-size: 2em;
        font-weight: bold;

        margin-right: 10px;
      }

      .top {
        align-self: flex-start;
        transform: translateY(200%);
      }
    }

    .right {
      ul {
        display: flex;
        justify-content: space-between;
        list-style: none;
        padding: 0;

        li {
          background: lightgrey;
          color: black;

          margin: 5px 10px;
          padding: 10px 20px;
          border-radius: 5px;
        }

        li:hover {
          background: grey;
        }

        li:first-child {
          margin-left: 0;
        }
        li:last-child {
          margin-right: 0;
        }
      }
    }
  }

  .days-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    text-align: center;
    margin: 5% 0;
    .day-card {
      width: 12%;

      h1 {
        margin: 0;
        font-size: 1em;
      }

      img {
        width: 100%;
      }

      span {
        font-size: 1em;
      }
    }
    .day-card:hover {
      border: 2px solid black;
      border-radius: 5px;
    }
  }
}

@media (max-width: 530px) {
  .home {
    padding: 5% 0;
    text-align: center;
    .contents {
      flex-direction: column;
      align-items: center;
    }
  }
}
</style>
