<template>
  <div class="menu">
    <!-- <a href="/#/ArraySort"><div class="item">ArraySort</div></a>
    <a href="/#/Clipboard"><div class="item">Clipboard转灰度图片</div></a>
    <a href="/#/Geolocation"><div class="item">Geolocation</div></a>
    <a href="/#/Keyboard"><div class="item">Keyboard</div></a>
    <a href="/#/WebRTC"><div class="item">WebRTC</div></a>
    <a href="/#/webRTCScreenShot"><div class="item">WebRTC屏幕截图</div></a>
    <a href="/#/shortUrl"><div class="item">ShortURL短网址系统</div></a>
    <a href="/#/webWorker"><div class="item">Web Worker</div></a>
    <a href="/#/ImageGrid"><div class="item">ImageGrid CSS grid</div></a>
    <a href="/#/LasyImage"><div class="item">LasyImage</div></a> -->
    <a href="/#/to64Tranform"><div class="item">To64Tranform</div></a>
    <a href="/#/toGray"><div class="item">上传并转灰度</div></a>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  components: {},
};
</script>

<style lang="less">
.menu {
  display: flex;
  align-items: center;
  flex-wrap: wrap;

  .item {
    border: 1px #e0e0e0 solid;
    background: #f0f0f0;
    width: 200px;
    height: 100px;
    margin: 10px;
    border-radius: 6px;
  }
}
</style>
