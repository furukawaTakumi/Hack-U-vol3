<template>
  <div>
    <h5 for="file-size" class="file-size-label">
      送信するファイルサイズを入力してください。
    </h5>
    <h3 class="file-size-text">{{ fileSizeText }}</h3>
    <div class="file-size-input">
      <input
        v-model.number="fileSizeNum"
        class="num-input"
        placeholder="10000"
        min="0"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fileSizeNum: 0,
      units: {
        byte: Math.pow(10, 0),
        KB: Math.pow(10, 3),
        MB: Math.pow(10, 6),
        GB: Math.pow(10, 9),
        TB: Math.pow(10, 12),
        PB: Math.pow(10, 15) 
      },
    }
  },
  methods: {
  },
  computed: {
    fileSizeText () {
      if ( '' === this.fileSizeNum || typeof this.fileSizeNum === 'string'){
        return '0 byte'
      } else if (Number.MAX_SAFE_INTEGER < this.fileSizeNum){
        return '扱える数値範囲を超えました。'
      } else if (this.units.PB <= this.fileSizeNum) {
        return `${(this.fileSizeNum / this.units.PB).toFixed(2)} PB`
      } else if (this.units.TB <= this.fileSizeNum) {
        return `${(this.fileSizeNum / this.units.TB).toFixed(2)} TB`
      } else if (this.units.GB <= this.fileSizeNum) {
        return `${(this.fileSizeNum / this.units.GB).toFixed(2)} GB`
      } else if (this.units.MB <= this.fileSizeNum) {
        return `${(this.fileSizeNum / this.units.MB).toFixed(2)} MB`
      } else if (this.units.KB <= this.fileSizeNum) {
        return `${(this.fileSizeNum / this.units.KB).toFixed(2)} KB`
      } 
      return `${this.fileSizeNum} byte`
    }
  }
}
</script>

<style lang="scss" scoped>
.file-size-text {
  text-align: center;
  font-size: 4em;
}
.file-size-input {
  display: flex;
  align-items: baseline;
  justify-content: center;
}
.file-size-label {
  font-size: 1.4em;
  padding: 20px;
  text-align: center;
}
.num-input {
  width: 400px;
  height: 3em;
  font-size: 1em;
  text-align: center;
  margin: 50px 0;
  background-color: aliceblue;
  border: solid 2px blue;
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
