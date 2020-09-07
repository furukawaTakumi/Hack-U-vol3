<template>
  <div>
    <!-- <ul>
      <li v-for="(post, index) in posts" :key="index">
        <a :href="'post.url'" target="_blank" rel="noopener noreferrer">{{ post.title }}</a>
      </li>
    </ul> -->
    <canvas id="target" />
    <button id="startButton" @click="test">
      計測開始
    </button>
    <a> {{ speed }} Mbps</a>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      speed: 0
    }
  },
  mounted () {
    const canvas = document.getElementById('target')
    const imagePath = 'dummy.png'
    draw(canvas, imagePath)
    function draw (canvas, imagePath) {
      console.log('draw')
      const image = new Image()
      image.addEventListener('load', function () {
        canvas.width = image.naturalWidth
        canvas.height = image.naturalHeight
        const ctx = canvas.getContext('2d')
        ctx.drawImage(image, 0, 0)
        console.log('load!')
      })
      image.src = imagePath
    }
  },
  methods: {
    async test () {
      this.inTestUI()

      // Canvasのデータをblob化
      const canvas = document.getElementById('target')
      const type = 'image/png'
      const dataurl = canvas.toDataURL(type)
      const bin = atob(dataurl.split(',')[1])
      const buffer = new Uint8Array(bin.length)
      for (let i = 0; i < bin.length; i++) {
        buffer[i] = bin.charCodeAt(i)
      }
      const blob = new Blob([buffer.buffer], { type })

      const data = new FormData()
      data.append('photo', blob, 'image.png')

      const sendDate = new Date().getTime()
      await axios.post('http://speedtest01.azurewebsites.net', data, {
        headers: { 'content-type': 'multipart/form-data' }
      })
        .then((res) => {
          // レスポンスがあった段階で計測
          const resDate = new Date().getTime()
          // Mbps単位で計算
          this.speed = (1800000 / ((resDate - sendDate) / (1000 * 2))) * 8 / 1000000
          // 切り捨て
          this.speed = this.speed.toFixed(2)
          console.log('success')
          this.initUI()
        }).catch((error) => {
          console.log('response error', error)
        })
    },
    inTestUI () {
      const button = document.getElementById('startButton')
      button.id = 'inTestButton'
      button.textContent = '計測中'
    },
    initUI () {
      const button = document.getElementById('inTestButton')
      button.id = 'startButton'
      button.textContent = '計測開始'
    }
  }
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
