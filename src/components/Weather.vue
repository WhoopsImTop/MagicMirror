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
import am4charts from '@amcharts/amcharts4/charts'
import am4core from '@amcharts/amcharts4/core'
import am4themes_animated from '@amcharts/amcharts4/themes/animated'

export default {
    name: "Weather",
    data() {
        return {
            data: [],
            current: []
        }
    },
    methods: {
        createSparkLine() {
            am4core.useTheme(am4themes_animated);

            var chart = am4core.create("chartdiv", am4charts.XYChart);

            chart.data = [ {
                    "date": new Date(2018, 0, 1, 8, 0, 0),
                    "value": 57
                }, {
                    "date": new Date(2018, 0, 1, 9, 0, 0),
                    "value": 27
                }, {
                    "date": new Date(2018, 0, 1, 10, 0, 0),
                    "value": 24
                }, {
                    "date": new Date(2018, 0, 1, 11, 0, 0),
                    "value": 59
                }, {
                    "date": new Date(2018, 0, 1, 12, 0, 0),
                    "value": 33
                }, {
                    "date": new Date(2018, 0, 1, 13, 0, 0),
                    "value": 46
                }, {
                    "date": new Date(2018, 0, 1, 14, 0, 0),
                    "value": 20
                }, {
                    "date": new Date(2018, 0, 1, 15, 0, 0),
                    "value": 42
                }, {
                    "date": new Date(2018, 0, 1, 16, 0, 0),
                    "value": 59,
                "opacity": 1
                }
            ];

            chart.padding(0, 0, 0, 0);

            var dateAxis = chart.xAxes.push(new am4charts.DateAxis());
            dateAxis.renderer.grid.template.disabled = true;
            dateAxis.renderer.labels.template.disabled = true;
            dateAxis.startLocation = 0.5;
            dateAxis.endLocation = 0.7;
            dateAxis.cursorTooltipEnabled = false;

            var valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
            valueAxis.min = 0;
            valueAxis.renderer.grid.template.disabled = true;
            valueAxis.renderer.baseGrid.disabled = true;
            valueAxis.renderer.labels.template.disabled = true;
            valueAxis.cursorTooltipEnabled = false;

            chart.cursor = new am4charts.XYCursor();
            chart.cursor.lineY.disabled = true;

            var series = chart.series.push(new am4charts.LineSeries());
            series.tooltipText = "{date}: [bold]{value}";
            series.dataFields.dateX = "date";
            series.dataFields.valueY = "value";
            series.tensionX = 0.8;
            series.strokeWidth = 2;

            // render data points as bullets
            var bullet = series.bullets.push(new am4charts.CircleBullet());
            bullet.circle.opacity = 0;
            bullet.circle.propertyFields.opacity = "opacity";
            bullet.circle.radius = 3;
        },
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
    font-size: 40px;
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