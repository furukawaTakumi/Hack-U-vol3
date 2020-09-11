<template>
  <div>
    <h1 class="result">
      <span style="font-color=blue">{{ result }}</span>
    </h1>
  </div>
</template>

<script>
export default {
  data: () => ({
    result: '結果待ち',
  }),
  methods: {
    resultJudge(fileSize, netSpeed, buturiTime) {
      if (fileSize === null || netSpeed === null || buturiTime === null) return
      const netTime = fileSize / (netSpeed * 1000000)
      console.log(netTime + ',' + buturiTime)
      if (netTime >= buturiTime) {
        this.result = '記録媒体を直接持って行った方が早いです'
      } else if (netTime < buturiTime) {
        this.result = 'アップロードした方が早いです'
      }
    },
  },
}
</script>

<style scoped>
.result {
  font-size: 10em;
  padding: 20px;
  text-align: center;
}
</style>
