<template>
  <div>
    <button @click="test">
      aaaa
    </button>
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  },
  methods: {
    test () {
      const ping = require('ping')
      console.log(ping)
      const hosts = ['192.168.1.22']
      hosts.forEach(function (host) {
        ping.sys.probe(host, function (isAlive) {
          const msg = isAlive ? 'host ' + host + ' is alive' : 'host ' + host + ' is dead'
          console.log(msg)
        })
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
