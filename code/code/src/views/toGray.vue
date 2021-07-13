<!--  -->
<template>
  <div>
    <input
      type="file"
      id="file"
      class="filepath"
      @change="changepic(this)"
      accept="image/jpg,image/jpeg,image/png,image/PNG"
    />
    <img src="" id="img" />
    <button @click="togray">保存为灰度图片</button>
  </div>
</template>

<script>
export default {
  name: "ToGray",
  data() {
    return {
        fileName:""
    };
  },

  components: {},

  computed: {},

  mounted() {},

  methods: {
    changepic() {
      var reads = new FileReader();
      let f = document.getElementById("file").files[0];
      this.fileName = f.name;
      reads.readAsDataURL(f);
      reads.onload = function(e) {
          console.log(e);
        document.getElementById("img").src = this.result;
      };
    },
    togray() {
      let imgObj = document.getElementById("img");
      var imgW = imgObj.offsetWidth;
      var imgH = imgObj.offsetHeight;
      let canvas = document.createElement("canvas");
      canvas.width = imgW;
      canvas.height = imgH;
      console.log("canvas", canvas);
      let canvasContext = canvas.getContext("2d");
      canvasContext.drawImage(imgObj, 0, 0);
      var imgPixels = canvasContext.getImageData(0, 0, imgW + 2, imgH + 2);
      console.log("wh", imgPixels, imgPixels.height, imgPixels.width);
      for (var y = 0; y < imgPixels.height; y++) {
        for (var x = 0; x < imgPixels.width; x++) {
          var i = y * 4 * imgPixels.width + x * 4;
          var avg =
            (imgPixels.data[i] +
              imgPixels.data[i + 1] +
              imgPixels.data[i + 2]) /
            3;

          imgPixels.data[i] = avg;
          imgPixels.data[i + 1] = avg;
          imgPixels.data[i + 2] = avg;
        }
      }
      canvasContext.putImageData(
        imgPixels,
        0,
        0,
        0,
        0,
        imgPixels.width,
        imgPixels.height
      );
      console.log(canvas.toDataURL());
      document.getElementById("img").src = canvas.toDataURL();
      var aLink = document.createElement('a');
      var blob = this.base64ToBlob(canvas.toDataURL()); //new Blob([content]);
      var evt = document.createEvent("HTMLEvents");
      evt.initEvent("click", true, true);//initEvent 不加后两个参数在FF下会报错  事件类型，是否冒泡，是否阻止浏览器的默认行为
      aLink.href = URL.createObjectURL(blob);
      // aLink.dispatchEvent(evt);
      aLink.download = this.fileName;
      aLink.click()
    },
    base64ToBlob(code) {
     var parts = code.split(';base64,');
     var contentType = parts[0].split(':')[1];
     var raw = window.atob(parts[1]);
     var rawLength = raw.length;
     
     var uInt8Array = new Uint8Array(rawLength);

     for (var i = 0; i < rawLength; ++i) {
         uInt8Array[i] = raw.charCodeAt(i);
     }
     return new Blob([uInt8Array], {type: contentType});
 }
  },
};
</script>
<style lang="less" scoped>
#img3 {
}
</style>
