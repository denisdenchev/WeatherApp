<template>
  <div
    id="app"
    :class=" typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''"
  >
    <main>
      <div id="searchBox">
        <input
          type="text"
          id="searchBar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div id="weather" v-if="typeof weather.main != 'undefined'">
        <div id="locationBox">
          <h2 id="location">{{weather.name}}</h2>
          <h3 id="date">{{ dateBuilder() }}</h3>
        </div>
        <div id="weatherBox">
          <h2 id="temperature">{{Math.round(weather.main.temp)}}°</h2>
          <h2 id="weather">{{ weather.weather[0].description }}</h2>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      apiKey: "0f33f5c78acf44e7d38b5f6706f6f59d",
      urlBase: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.urlBase}weather?q=${this.query}&appid=${this.apiKey}&units=metric`
        )
          .then(res => {
            return res.json();
          })
          .then(res => {
            this.setResults(res);
          });
      }
    },
    setResults(results) {
      this.weather = results;
      // console.log(results);
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
        "December"
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, Helvetica, sans-serif;
}

#app {
  background-image: url("./assets/cold.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.55)
  );
}

#searchBox {
  width: 100%;
  margin-bottom: 30px;
}

#searchBox #searchBar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
}

#searchBox #searchBar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  border-radius: 16px 0;

  background-color: rgba(255, 255, 255, 0.75);
}

#locationBox #location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

#locationBox #date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

#weatherBox {
  text-align: center;
}

#weatherBox #temperature {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.45);
  border-radius: 26px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}

#weatherBox #weather {
  font-size: 48px;
  color: #fff;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}
</style>
