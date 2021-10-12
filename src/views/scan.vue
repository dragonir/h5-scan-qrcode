<template>
  <div class="join-panel join_by_scan">
    <div class="nav">
      <a class="_close" @click="() => $router.go(-1)"></a>
      <p class="_title">扫一扫</p>
    </div>
    <div class="scroll-container">
      <Scaner
        v-on:code-scanned="codeScanned"
        v-on:error-captured="errorCaptured"
        :stop-on-scanned="true"
        :draw-on-found="true"
        :responsive="false"
      />
    </div>
  </div>
</template>

<script>

import Scaner from '../components/Scaner';

export default {
  name: 'joinByScan',
  components: {
    Scaner
  },
  data () {
    return {
      errorMessage: "",
      scanned: ""
    }
  },
  methods: {
    codeScanned(code) {
      this.scanned = code;
      window.open(code, '_blank');
    },
    errorCaptured(error) {
      switch (error.name) {
        case "NotAllowedError":
          this.errorMessage = "Camera permission denied.";
          break;
        case "NotFoundError":
          this.errorMessage = "There is no connected camera.";
          break;
        case "NotSupportedError":
          this.errorMessage =
            "Seems like this page is served in non-secure context.";
          break;
        case "NotReadableError":
          this.errorMessage =
            "Couldn't access your camera. Is it already in use?";
          break;
        case "OverconstrainedError":
          this.errorMessage = "Constraints don't match any installed camera.";
          break;
        default:
          this.errorMessage = "UNKNOWN ERROR: " + error.message;
      }
      console.error(this.errorMessage);
     alert('相机调用失败，请切换其他浏览器或使用企业码加入云盘');
    }
  },
  mounted () {
    var str = navigator.userAgent.toLowerCase(); 
    var ver = str.match(/cpu iphone os (.*?) like mac os/);
    if (ver && ver[1].replace(/_/g,".") < '10.3.3') {
     alert('相机调用失败，请切换其他浏览器或使用企业码加入云盘');
    }
  }
}
</script>

<style lang="css" scoped>
.join_by_scan {
  height: 100%;
  width: 100%;
}
</style>