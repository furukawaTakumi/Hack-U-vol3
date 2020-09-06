<template>
  <div>
    <div id="ping-github"></div>
    <button @click="test">
      aaaa
    </button>
  </div>
</template>

<script>
import Ping from 'ping.js'

export default {
  data () {
    return {
    }
  },
  methods: {
    test () {
      const p = new Ping()

      p.ping('https://google.com', function (err, data) {
        // Also display error if err is returned.
        if (err) {
          console.log('error loading resource')
          data = data + ' ' + err
        }
        document.getElementById('ping-github').innerHTML = data
      })
    }
    // ping () {
    //   const axios = require('axios')
    //   const url = ''

    //   const startTime = Date.now()

    //   axios.get(url).then((res) => {
    //     // const items = res.data
    //     const endTime = Date.now()

    //     console.log(endTime - startTime)

    //     const t = res
    //     alert(t)
    //   }).catch((error) => {
    //     const {
    //       status,
    //       statusText
    //     } = error.response
    //     console.log(`Error! HTTP Status: ${status} ${statusText}`)
    //   })
    // }
  }
}
</script>
