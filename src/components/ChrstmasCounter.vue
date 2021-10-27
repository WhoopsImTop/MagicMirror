<template>
  <div class="christmas-countdown">
      <h1 style="font-size: 20px; color: #9e9e9e; letter-spacing: 5px; font-weight: 600">COUNTDOWN IN</h1>
      <span style="font-size: 50px; font-weight: 600">{{daysLeft}}<span class="Countdown-indicator">T</span> {{hrsLeft}}<span class="Countdown-indicator">H</span> {{minsLeft}}<span class="Countdown-indicator">M</span> {{seksLeft}}<span class="Countdown-indicator">S</span></span>
      <h1 style="font-size: 20px; color: #9e9e9e; letter-spacing: 5px; font-weight: 600">IST WEIHNACHTEN</h1>
  </div>
</template>

<script>
export default {
    name: "ChristmasCounter",
    data() {
        return {
            daysLeft: "",
            hrsLeft: "",
            minsLeft: "",
            seksLeft: "",
        }
    },
    methods: {
        CalculateCountdown() {
            var today = new Date();
            var target = new Date(`December 24, ${today.getFullYear()} 00:00:00`);
            var currentTime = today.getTime();
            var targetTime = target.getTime();
        
            var time = targetTime - currentTime;
        
            var seksLeft = Math.floor(time/1000);
            var minsLeft = Math.floor(seksLeft/60);
            var hrsLeft = Math.floor(minsLeft/60);
            this.daysLeft = Math.floor(hrsLeft/24);
        
            hrsLeft = hrsLeft % 24;
            minsLeft = minsLeft % 60;
            seksLeft = seksLeft % 60;
        
        
            this.hrsLeft = (hrsLeft<10) ? "0"+hrsLeft : hrsLeft;
            this.minsLeft = (minsLeft<10) ? "0"+minsLeft : minsLeft;
            this.seksLeft = (seksLeft<10) ? "0"+seksLeft : seksLeft;
        
            if(time > 0){
                setTimeout(this.CalculateCountdown, 1000)
            }
        }
    },
    created() {
        this.CalculateCountdown()
    }
}
</script>

<style>
.christmas-countdown {
    color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    min-width: 500px;
}

.christmas-countdown > h1 {
    margin: 0;
    padding: 0;
}

.Countdown-indicator {
    color: #9e9e9e;
    font-weight: 400;
}
</style>