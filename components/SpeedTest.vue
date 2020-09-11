<template>
  <div>
    <canvas id="target" />
    <button id="startButton" @click="start">計測開始</button>
    <a> {{ valueSpeed }} Mbps</a>
    <v-container
      light-blue--text
      text--lighten-4
      text-center
      @click="isOpenedModal = !isOpenedModal"
    >
      △スピードテストの詳細を表示する
    </v-container>
    <v-dialog v-model="isOpenedModal" class="detail-modal">
      <v-card>
        <v-card-title>スピードテスト詳細</v-card-title>
        <v-card-text>
          <v-simple-table>
            <tr>
              <td>テストサイズ</td>
              <td>{{ testSize }}MB</td>
            </tr>
            <tr>
              <td>テスト回数</td>
              <td>{{ testCnt }}/{{ maxTest }}回(現在/最大)</td>
            </tr>
          </v-simple-table>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      speed: 0,
      speeds: [],
      valueSpeed: 0,
      inTest: false,
      testCnt: 0,
      maxTest: 5,
      testServer: 'http://speedtest02.azurewebsites.net',
      subTestServer: 'http://speedtest01.azurewebsites.net',
      isOpenedModal: false,
      testSize: 0,
      maxSizeKB: 32000,
      errorText: 'テスト中です',
    }
  },
  methods: {
    start() {
      if (this.inTest) {
        return
      }
      this.test()
    },
    async test() {
      if (this.testCnt === 0) {
        this.inTestUI()
        this.inTest = true
      }

      const type = 'image/png'
      let bin = ''
      // 適当な文字列を生成
      for (let j = 0; j < this.maxSizeKB; j++) {
        for (let i = 0; i < 100; i++) {
          bin = bin + 'aaaaaaaaaa'
        }
      }

      const buffer = new Uint8Array(bin.length)
      for (let i = 0; i < bin.length; i++) {
        buffer[i] = bin.charCodeAt(i)
      }
      const blob = new Blob([buffer.buffer], { type })
      const data = new FormData()
      data.append('photo', blob, 'image.png')
      console.log('post start(' + buffer.length / 1000000 + 'MB)')
      this.testSize = buffer.length / 1000000
      // postの時間
      const sendDate = new Date().getTime()
      // 画像としてURLに載せてpost
      await axios
        .post(this.testServer, data, {
          headers: { 'content-type': 'multipart/form-data' },
        })
        .then(() => {
          // レスポンスがあった段階で計測
          const resDate = new Date().getTime()
          // Mbps単位で計算
          // this.speed = (buffer.length / ((resDate - sendDate) / (1000 * 2))) * 8 / 1000000
          const _byte = buffer.length
          const _bit = _byte * 8
          const _mb = _bit / 1000000
          const _msTime = (resDate - sendDate) / 1000
          const _toServerTime = _msTime / 2
          // mbps単位
          const currentSpeed = _mb / _toServerTime

          if (this.testCnt !== 0) {
            this.speeds.push(currentSpeed)

            let total = 0

            for (let i = 0; i < this.speeds.length; i++) {
              total += this.speeds[i]
            }

            this.speed = total / this.speeds.length
          }

          // 切り捨て
          this.valueSpeed = this.speed.toFixed(2)
          console.log(
            'post success(cnt' +
              ': ' +
              this.testCnt +
              ' ' +
              currentSpeed +
              '>' +
              this.speed +
              ')'
          )

          if (this.maxTest - 1 >= this.testCnt) {
            this.testCnt++
            this.test()
          } else {
            this.initUI()
            this.testCnt = 0
            this.inTest = false
            this.$emit('finish-speed-test', this.speed)
          }
        })
        .catch((error) => {
          if (this.testServer !== this.subTestServer) {
            console.log('change server >' + this.subTestServer)
            this.testServer = this.subTestServer
            this.test()
          } else {
            this.initUI()
            this.inTest = false
            this.testCnt = 0
            console.log('response error', error)
          }
        })
    },
    inTestUI() {
      const button = document.getElementById('startButton')
      button.id = 'inTestButton'
      button.textContent = '計測中'
    },
    initUI() {
      const button = document.getElementById('inTestButton')
      button.id = 'startButton'
      button.textContent = '計測開始'
    },
  },
}
</script>

<style scoped>
#target {
  display: none;
}
#startButton {
  background-color: #00a656;
  border-radius: 1.5em;
  box-shadow: 0 0.2em 0.5em rgba(0, 0, 0, 0.2);
  padding: 1em 2em;
  color: #ffffff;
  font-weight: bold;
  text-decoration: none;
}
#inTestButton {
  background-color: #eb4200;
  border-radius: 1.5em;
  box-shadow: 0 0.2em 0.5em rgba(0, 0, 0, 0.2);
  padding: 1em 2em;
  color: #ffffff;
  font-weight: bold;
  text-decoration: none;
}
</style>
