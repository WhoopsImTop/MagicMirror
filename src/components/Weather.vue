<template>
  <div>
      <div class="weather-container">
          <div class="weather-main-info">
                <img style="width: 150px; height: 150px;" :src="'../static/' + current.weather[0].icon + '.svg'"/>
                <div>
                    <p class="temperature">{{ current.main.temp }} °C</p>
                    <p style="margin: 0; padding: 0">{{current.weather[0].description}}</p>
                </div>
                <div id="chartdiv"></div>
          </div>
          <hr style="width: 100%; opacity: .5">
          <div class="weather-info-next">
              <div class="next-info" v-for="n in 5" :key="data[n].dt">
                  <img :src="'../static/' + data[n].weather[0].icon + '.svg'"/>
                  <p style="font-weight: 600">{{roundNumbers(data[n].main.temp)}} °C</p>
                  <p style="opacity: .5">{{SpliceTime(data[n].dt_txt)}}</p>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: "Weather",
    data() {
        return {
            data: [],
            current: []
        }
    },
    methods: {
        SpliceTime(str) {
            return str.substr(11).slice(0, -3)
        },
        roundNumbers(number) {
            return Math.round(number)
        },
        getCurrentTemp() {
            axios
            .get('https://api.openweathermap.org/data/2.5/weather?zip=79100,de&units=metric&lang=de&appid=96b97ba09d630bf742fc82a2b5d71a80')
            .then(Response => {
                this.current = Response.data
            })
            .catch((e) => {
                console.log("Keine verbundung herstellbar", e)
            })
            setTimeout(this.getCurrentTemp, 5*60*1000)
        },
        getWeatherData() {
            axios
            .get('https://api.openweathermap.org/data/2.5/forecast?zip=79100,de&units=metric&lang=de&appid=96b97ba09d630bf742fc82a2b5d71a80')
            .then(Response => {
                this.data = Response.data.list
            })
            .catch((e) => {
                console.log("Keine verbundung herstellbar", e)
            })
            setTimeout(this.getWeatherData, 30 * 60 * 1000)
        }
    },
    created() {
        this.getWeatherData()
        this.getCurrentTemp()
    }
}
</script>

<style>
.weather-container {
    display: flex;
    flex-direction: column;
    color: #efefef;
    width: 380px;
}

.weather-main-info {
    display: flex;
    justify-content: center;
    align-items: center;
}

.temperature {
    font-size: 30px;
    font-weight: 600;
    padding: 0;
    margin: 0;
    height: 50px;
}

.next-info {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border: 1px solid #efefef;
    border-radius: 5px;
    margin: 0px 5px;
    padding: 0px 0px 10px 0px
}


.weather-info-next {
    display: flex;
    align-items: center;
}

.next-info:first-child {
    background-color: #ffffff20;
}

.next-info > p {
    margin: 0;
    margin-bottom: -5px;
    padding: 0;
}

#chartdiv {
  width: 200px;
  height: 30px;
  border: 1px dotted #eee;
}
</style>