<template>
  <div class="home">
    <i class="banner"></i>
    <div class="tips">
      <p class="tips_title">HELLO WORLD</p>
      <p class="tips_descript">Scan QRcode By Your Browser</p>
    </div>
    <button class="button" @click="$router.push({name: 'Scan'})">SCAN QRCODE</button>
  </div>
</template>

<script>

export default {
  name: 'Home'
}
</script>

<style>
.home {
  height: 100vh;
  background: #5F68E8;
  position: relative;
}
.home .banner {
  display: inline-block;
  width: 100%;
  height: 0px;
  padding-top: 150%;
  background: url('../assets/bg.png') no-repeat center;
  background-size: auto 100%;
  animation: move 5s ease-in-out infinite;
  animation-fill-mode: both;
}
.tips {
  width: 100%;
  position: absolute;
  bottom: 180px;
  left: 0;
  padding-top: 400px;
  color: #FFFFFF;
}
.tips p {
  padding: 0;
  margin: 0;
}
.tips .tips_title {
  font-size: 32px;
  font-weight: bold;
  text-shadow: 0px 1px 1px rgba(0, 0, 0, .15);
}
.tips .tips_descript {
  font-size: 20px;
  padding-top: 12px;
}
.button {
  height: 56px;
  width: 246px;
  line-height: 56px;
  background: url('../assets/button.png') no-repeat center;
  background-size: 100% 100%;
  position: absolute;
  bottom: 72px;
  left: 50%;
  margin-left: -123px;
  outline: none;
  border: none;
  -webkit-appearance: none;
  user-select: none;
  color: #FFFFFF;
  font-size: 18px;
  filter: drop-shadow(1px 1px 5px rgba(0, 0, 0, .25));
}

@keyframes move {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(36px);
  }
  100% {
    transform: translateY(0px);
  }
}
</style>
