<template>
  <div class="home">
    <div class="tips">
      <p class="tips_title">HELLO WORLD</p>
      <p class="tips_descript">hello world hello world hello</p>
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
  background: #5F68E8 url('../assets/bg.png') no-repeat center;
  background-size: auto 100%;
  position: relative;
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
</style>
