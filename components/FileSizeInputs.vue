<template>
  <div>
    <h5 for="file-size" class="file-size-label">
      送信するファイルサイズを入力してください。
    </h5>
    <div class="file-size-input">
      <input
        v-model="fileSizeNum"
        id="file-size"
        type="number"
        class="num-input"
        min="0"
      />
      <div class="unit-type">{{ unitType.text }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fileSizeNum: 1,
      unitType: { id: 0, text: 'byte', value: 1 },
      unitTypes: [
        { id: 0, text: 'byte', value: 1 },
        { id: 1, text: 'KB', value: 1000 },
        { id: 2, text: 'MB', value: 1000000 },
        { id: 3, text: 'GB', value: 1000000000 },
        { id: 4, text: 'TB', value: 1000000000000 },
        { id: 5, text: 'PB', value: 1000000000000000 },
      ],
    }
  },
}
</script>

<style lang="scss" scoped>
.file-size-input {
  display: flex;
  align-items: baseline;
  justify-content: center;
}
@mixin not-outline {
  outline: none;
  &:hover {
    outline: none;
    background-color: aquamarine;
  }
  &:active {
    outline: none;
  }
}
.file-size-label {
  font-size: 1.4em;
  padding: 20px;
  text-align: center;
}

.num-input {
  @include not-outline;
  height: 80px;
  width: 200px;
  font-size: 4.5em;
  text-align: right;
  margin: 50px 0;
}
input[type='number']::-webkit-inner-spin-button,
input[type='number']::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.unit-type {
  height: 1.2em;
  width: 100px;
  font-size: 3em;
  margin-left: 0.4rem;
}
</style>
