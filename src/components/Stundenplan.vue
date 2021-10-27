<template>
  <div class="stundenplan-container" v-show="stundenplanData.length != 0">
      <h2 style="font-size: 20px; letter-spacing: 5px; color: #9e9e9e; font-weight: 600">STUNDENPLAN</h2>
      <div class="tag-container">
          <div :id="tage.name" v-for="tage in Wochentage" :key="tage.name" v-show="tage.name === Wochentage[getToday()].name">
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            stundenplanData: [],
            Wochentage: [
                {
                    name: "Sonntag",
                    kürzel: "MO"
                },
                {
                    name: "Montag",
                    kürzel: "MO"
                },
                {
                    name: "Dienstag",
                    kürzel: "DI"
                },
                {
                    name: "Mittwoch",
                    kürzel: "MI"
                },
                {
                    name: "Donnerstag",
                    kürzel: "DO"
                },
                {
                    name: "Freitag",
                    kürzel: "FR"
                },
                {
                    name: "Samstag",
                    kürzel: "MO"
                },
            ]
        }
    },
    methods: {
        getToday() {
            let date = new Date();
            return date.getDay()
        },
        fetchData() {
            axios
            .get('http://192.168.178.81:3004/api/stundenplan')
            .then(response => {
                this.stundenplanData = response.data
                this.SortData()
                this.getToday()
            })
            .catch((e) => {
                console.log("Daten konnten nicht geladen werden", e)
            })
        },
        SortData() {
            let data = this.stundenplanData
            for(let i = 0; i < data.length; i++) {
                let container = document.getElementById(data[i].tag)
                let appendAble = `<div class="stunde" style="min-height: ${data[i].zeitraum}px"><span style="font-weight: 600">${data[i].fach}</span><span style="color: #9e9e9e">${data[i].dozent}</span></div>`
                container.innerHTML += appendAble
            }
            for(let x = 0; x < this.Wochentage.length; x++) {
                if(document.getElementById(this.Wochentage[x].name).childElementCount === 0) {
                    let appendAble = '<div class="stunde">Freier Tag</div>'
                    let container = document.getElementById(this.Wochentage[x].name)
                    container.innerHTML += appendAble
                }
            }
        }
    },
    created() {
        this.fetchData()
    }
}
</script>

<style>
.stundenplan-container {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    color: #efefef;
    display: flex;
    flex-direction: column;
}

.stundenplan-container > h2 {
    margin: 0;
    padding: 0;
}

.tag-container {
    display: flex;
    flex-direction: column;
    padding: 10px 10px;
}

.tag-container > h3 {
    margin: 0;
    padding: 0;
}

.stunde {
    display: flex;
    flex-direction: column;
    justify-content: center;
    border-radius: 5px;
    border: 1px solid #fff;
    padding: 10px 10px;
    max-width: 200px;
    align-items: center;
    word-break: break-all;
    margin: 10px 0;
}

.stunde > span {
    margin: 0;
    padding: 0;
    display: inline-block;
}
</style>