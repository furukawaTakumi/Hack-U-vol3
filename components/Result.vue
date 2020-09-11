<template>
  <div class="frame">
    <v-app id="inspire">
      <v-data-table
        :headers="headers"
        :items="times"
        hide-default-header
        hide-default-footer
        class="elevation-1"
        width="320px"
      />
      <h1 class="result">
        <span style="font-color=blue">{{ result }}</span>
      </h1>
    </v-app>
  </div>
</template>

<script>
export default {
  data: () => ({
    result: '結果待ち',
    netTime: 0,
    buturiTime: 0,
    headers: [
      { text: 'aaaa', value: 'name' },
      { text: 'bbbb', value: 'time' },
    ],
    times: [
      {
        name: '移動時間(物理)',
        time: '',
      },
      {
        name: 'ネットワーク転送時間',
        time: '',
      },
    ],
  }),
  methods: {
    resultJudge(fileSize, netSpeed, buturiTime) {
      if (fileSize === null || netSpeed === null || buturiTime === null) return
      this.buturiTime = buturiTime
      this.netTime = fileSize / (netSpeed * 1000000)
      this.netTime = parseInt(this.netTime)
      console.log(this.netTime + ',' + buturiTime)
      this.times[0].time = this.time2Text(this.buturiTime)
      this.times[1].time = this.time2Text(this.netTime)
      if (this.netTime >= buturiTime) {
        this.result = '記録媒体を直接持って行った方が早いでしょう'
      } else if (this.netTime < buturiTime) {
        this.result = 'ネットワーク転送した方が早いでしょう'
      }
    },
    sec2hour(time) {
      var sec = (time % 60) % 60
      var min = Math.floor(time / 60) % 60
      var hour = Math.floor(time / 3600)
      return {
        hour: hour,
        min: min,
        sec: sec,
      }
    },
    sec2min(time) {
      var min = Math.floor(time / 60)
      var sec = time % 60

      return {
        min: min,
        sec: sec,
      }
    },
    hour2day(overHour) {
      var hour = Math.floor(overHour.hour / 24)
      var day = overHour.hour % 24

      return {
        hour: hour,
        day: day,
      }
    },
    time2Text(time) {
      if (time < 60) {
        return time + '秒'
      } else if (time >= 60 && time < 3600) {
        time = this.sec2min(time)
        return time.min + '分' + time.sec + '秒'
      } else if (time >= 3600 && time < 86400) {
        time = this.sec2hour(time)
        return time.hour + '時間' + time.min + '分' + time.sec + '秒'
      } else if (time >= 86400) {
        time = this.sec2hour(time)
        const dayhour = this.hour2day(time)
        return (
          dayhour.day +
          '日と' +
          dayhour.hour +
          '時間' +
          time.min +
          '分' +
          time.sec +
          '秒'
        )
      }
    },
  },
}
</script>

<style scoped>
.result {
  font-size: 5em;
  padding: 20px;
  text-align: center;
}
</style>
