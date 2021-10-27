<template>
  <div class="clock-container">
    <p style="font-weight: 600; letter-spacing: 5px" v-html="weekday"></p>
    <h2 v-html="time" class="clock"></h2>
    <p v-html="date"></p>
  </div>
</template>

<script>
export default {
  name: 'Clock',
  data() {
    return {
    time: '',
    date: '',
    weekday: '',
    week: ['SONNTAG', 'MONTAG', 'DIENSTAG', 'MITTWOCH', 'DONNERSTAG', 'FREITAG', 'SAMSTAG'],
    }
  },
  methods: {
    updateTime() {
      let date = new Date();
      this.time = this.zeroPadding(date.getHours(), 2) + ":" + this.zeroPadding(date.getMinutes(), 2) + ':' + '<span style="font-weight: 300; opacity: .5">' + this.zeroPadding(date.getSeconds(), 2) + '</span>';
      this.date;
      if((this.zeroPadding(date.getDate(), 2) + '.' + this.zeroPadding(date.getMonth()+1, 2) + '.' + date.getFullYear(), 4) === `26.06.${date.getFullYear(), 4}`) {
        this.date = "Happy Birthday &#128149;"
      } else {
        this.date = this.zeroPadding(date.getDate(), 2) + '.' + this.zeroPadding(date.getMonth()+1, 2) + '.' + date.getFullYear(), 4;
      }
      this.weekday = this.week[date.getDay()]
      setTimeout(this.updateTime, 1000)
    },
    zeroPadding(num, digit) {
      var zero = '';
      for(var i = 0; i < digit; i++) {
          zero += '0';
      }
      return (zero + num).slice(-digit);
    }
  },
  created() {
    this.updateTime()
  }
}
</script>

<style scoped>
  .clock-container {
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    align-items: center;
  }
  .clock {
    color: #efefef;
    font-weight: 600;
    font-size: 50px;
    padding: 0;
    margin: 0;
    min-width: 220px;
    text-align: center;
  }
  h2 {
    padding: 0px;
    margin: 0px;
  }
  p {
    color: #9e9e9e;
    font-size: 20px;
    font-weight: 400;
    margin: 0;
    padding: 0;
  }
</style>
