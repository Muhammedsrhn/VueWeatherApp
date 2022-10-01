<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class=" search-box">
        <input type="search" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather()" />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country }}</div>
          <div class="date">{{getDate()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp-273.15)}}°c </div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
      <div class="not-found" v-else-if="weather.cod">No Match Found</div>
    </main>
  </div>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: "you-api-key",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {

      },
    }
  },

  methods: {
    fetchWeather() {
      setTimeout(() => {
        fetch(`${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          })
          .then(result => {
            this.weather = result;
            console.log(this.weather)
          });
      })

    },
    getDate() {
      const d = new Date();
      const days = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];
      const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]

      let day = days[d.getDay()];
      let month = months[d.getDay()];
      let date = d.getDate();
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}


body {
  font-family: 'Courier New', Courier, monospace;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 1.5rem;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: flex;
  width: 100%;
  padding: 0.7rem;
  font-size: 1.5rem;
  border-radius: 0px 16px 0px 16px;
  border: 1px solid black;
  outline: none;
  color: #313131;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
}

.search-box .search-bar:focus {
  border-radius: 16px 0px 16px 0px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}

@media only screen and (max-width: 764px) {
  .search-box .search-bar {
    width: 90%;
  }

}

.location-box .location {
  color: #fff;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 1.5rem;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(255, 255, 255, 0.25);
  font-weight: 900px;
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.not-found {
  color: #fff;
  font-size: 50px;
  text-align: center;
  font-style: italic;
  font-weight: 700;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);


}
</style>
