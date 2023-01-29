<template>
  <div class="wrapper">
    <div class="main">
      <h1>Weather app</h1>
      <h4>Check the weather in {{city == "" ? "your town" : "«" + city + "»"}}</h4>
      <div><input type="text" v-model="city" placeholder="Enter city">
      <button v-show="city != ''" @click="getWeather()">Get weather</button></div>
      <p class="error">{{ error }}</p>
    </div>

    <aside>
      <div v-if="info != null" class="info">
        <p>{{ info }}</p>
        <p>{{ showTemp }}</p>
        <p>{{ showFeelsLike }}</p>
        <p>{{ showMinTemp }}</p>
        <p>{{ showMaxTemp }}</p>
      </div>
    </aside>
  </div>
  <img src="./assets/img/g.png" alt="">
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    showTemp() {
      return "Temperature: " + this.info.main.temp
    },
    showFeelsLike() {
      return "As if: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Minimum temperature: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Maximum temperature: " + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "More than one character name required"
        return false
      }

      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=5aca7063330b959340ff2f185bc13176`)//&appid=API key
        .then(res => (this.info = res.data))
    }
  }
}
</script>

<style scoped>
.wrapper {
  display: flex;
  justify-content: flex-end;
  width: 60vw;
  height: 90vh;
  color: rgb(0, 0, 0);
  filter: blur(20px);
  background: #ffffff20;
  border: 2px solid #646464;
}

img {
  bottom: 0;
  width: 100%;
  position: absolute;
  left: 0;
  opacity: .8;
}

.wrapper .main {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 60%;
}

.wrapper aside {
  width: 30%;
  background-color: #6e2d7d;
  float: right;
}

.wrapper h1 {
  margin-top: 50px;
  margin-bottom: 20px;
}

.wrapper h4 {
  margin-bottom: 20px;
}

.wrapper p {
  margin-top: 20px;
  color: #e3d2d2;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #6e2d7d;
  color: #000000;
  font-size: 14px;
  padding: 5px 1px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background: #e3bc4d;
  color: rgb(0, 0, 0);
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.error {
  color: #d03939;
}
</style>
