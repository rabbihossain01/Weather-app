<template>
  <div id="app">
    <main
      :class="{
        warm: isWarm,
        clouds: isCloud,
        clear: isClear,
        haze: isHaze,
        rain: isRain,
        cold: isCold,
      }"
    >
      <h1>Weather App</h1>
      <div class="search-box">
        <input
          v-model="query"
          @keypress="fetchWeather"
          type="text"
          class="search-bar"
          placeholder="Search your city..."
        />
      </div>
      <div class="weather-wrap" v-if="weather">
        <div class="location-box">
          <div class="location">
            {{ weather.name }},{{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
         <div class="text">
             <ul id="w-details" class="list-group mt-3">
                    <li class="list-group-item">{{`Relative Humidity: ${weather.main.humidity +'%'}`}}</li>
                    <li class="list-group-item">{{`Feels Like: ${weather.main.feels_like}`}}</li>
                    <li class="list-group-item">{{`Pressure: ${weather.main.pressure +"°"}`}}</li>
                    <li class="list-group-item">{{`Wind: ${weather.wind.speed +'km/h'}`}}</li>
                     
                </ul>
         </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "f064a082d05edbbddf41d7254c8176ed",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: null,
    };
  },
  computed: {
    isWarm() {
      return this.weather ? this.weather.weather[0].main === "Warm" : null;
    },
    isCloud() {
      return this.weather ? this.weather.weather[0].main === "Clouds" : null;
    },
    isHaze() {
      return this.weather ? this.weather.weather[0].main === "Haze" : null;
    },
    isRain() {
      return this.weather ? this.weather.weather[0].main === "Rain" : null;
    },
    isClear() {
      return this.weather ? this.weather.weather[0].main === "Clear" : null;
    },
    isCold() {
      return this.weather ? this.weather.weather[0].main === "cold" : null;
    },
  },
  mounted() {
    fetch(`${this.url_base}weather?q=Dhaka&units=metric&APPID=${this.api_key}`)
      .then((res) => {
        return res.json();
      })
      .then(this.setResults);
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
      this.weather = results;
      console.log(results);
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

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: "Montserrat", sans-serif;
}

main {
  background-image: url("assets/cold.jpeg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
h1 {
  text-align: center;
  margin: 1rem auto;
  color: rgba(251, 253, 251, 0.541);
  font-style: italic;
  font-size: 4rem;
  font-weight: 700;
}
.search-box {
  max-width: 600px;
  margin-bottom: 30px;
  margin: auto;
 
}
.search-bar {
  width: 90%;
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  border-radius: 5px 15px 5px 15px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}
:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.75);
  color: #fff;
}
.weather-wrap {
  color: #fff;
  font-size: 50px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin-top: 2rem;
}

.date {
  color: #fff;
  font-size: 40px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  margin-top: 1rem;
}
.weather-box {
  text-align: center;
}
.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-weight: 900;
  font-size: 102px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.rain {
  background-image: url("assets/rain.jpg");
}
.warm {
  background-image: url("assets/warm-bg.jpg");
}
.cold {
  background-image: url("assets/cold.jpeg");
}
main {
  background-image: url("assets/cloudy.jpg");
}
.haze {
  background-image: url("assets/haze.jpg");
}
.clear {
  background-image: url("assets/clear.jpg");
}

.text {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-weight: 900;
  font-size: 102px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);

}

.list-group-item{
   color: #fff;
  font-size: 48px;
  font-weight: 700px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);

}

ul li {
  text-align: center;
  list-style: none;
  margin: auto;
  padding: 0.2rem;
}
</style>