<template>
  <div />
</template>

<script>
export default {
  async asyncData ({ $axios }) {
    // 取得先のURL
    const url = 'https://qiita.com/api/v2/items'
    // リクエスト（Get）
    const response = await $axios.$get(url)
    // 配列で返ってくるのでJSONにして返却
    return {
      posts: response
    }
  }
}
</script>
