<template>
  <div id="arrowAnim">
    <div class="arrowSliding">
      <div class="arrow" />
    </div>
    <div class="arrowSliding delay1">
      <div class="arrow" />
    </div>
    <div class="arrowSliding delay2">
      <div class="arrow" />
    </div>
    <div class="arrowSliding delay3">
      <div class="arrow" />
    </div>
  </div>
</template>

<script>
export default {
}

</script>

<style scoped>
#arrowAnim {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.arrow {
  width: 5vw;
  height: 5vw;
  border: 1.5vw solid;
  border-color: black transparent transparent black;
  transform: rotate(135deg);
}

.arrowSliding {
  position: absolute;
  -webkit-animation: slide 1s linear infinite;
          animation: slide 1s linear infinite;
          animation-direction:normal;
}

.delay1 {
  -webkit-animation-delay: 1s;
    animation-delay: 1s;
}
.delay2 {
  -webkit-animation-delay: 1s;
    animation-delay: 1s;
}
.delay3 {
  -webkit-animation-delay: 1s;
    animation-delay: 1s;
}

@-webkit-keyframes slide {
    0% { opacity:0; transform: translateX(-17vw); }
   20% { opacity:1; transform: translateX(-9vw); }
   80% { opacity:1; transform: translateX(9vw); }
  100% { opacity:0; transform: translateX(17vw); }
}
@keyframes slide {
    0% { opacity:0; transform: translateX(-17vw); }
   20% { opacity:1; transform: translateX(-9vw); }
   80% { opacity:1; transform: translateX(9vw); }
  100% { opacity:0; transform: translateX(17vw); }
}
</style>
