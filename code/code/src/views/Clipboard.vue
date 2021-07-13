<template>
  <div class="page">
    Clipboard
    <hr />
    <section>复制内容:{{ pasteContent }}<br /></section>
    <section>
      粘贴内容:
      <textarea> </textarea>
    </section>

    <section>
      粘贴图片:
      <img id="pic" ref="img" :src="pasteImage" />
    </section>
    <button @click="togray">保存为灰度图片</button>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Keyborad",
  data() {
    return {
      copyContent: "",
      pasteContent: "",
      pasteImage: "",
    };
  },
  mounted() {
    this.toFixed(1.005, 2);

    document.body.oncopy = (evt) => {
      const selection = document.getSelection();
      evt.clipboardData.setData(
        "text/plain",
        "该文档不允许复制剪贴操作，谢谢配合!" + selection.toString()
      );
      evt.preventDefault();
    };

    document.body.onpaste = (evt) => {
      console.log("evt", evt);
      this.pasteContent = evt.clipboardData.getData("text/plain");

      if (evt.clipboardData.items) {
        evt.clipboardData.items.forEach((item) => {
          if (item.type.includes("image")) {
            let file = item.getAsFile();
            console.log(file);

            var reader = new FileReader();
            reader.onload = (e) => {
              // event.target.result就是图片的Base64地址啦
              this.$refs.img.style.width = file.width + "px";
              this.$refs.img.style.height = file.height + "px";
              this.pasteImage = e.target.result;
              console.log(this.pasteImage);
            };
            reader.onerror = (err) => {
              console.log(err);
            };
            reader.readAsDataURL(file);
          }
        });
      }
    };
  },

  methods: {
    toFixed(num, s) {
      var times = Math.pow(10, s);
      var des = num * times + 0.5;
      des = parseInt(des, 10) / times;
      return des + "";
    },
    togray() {
      let imgObj = document.getElementById("pic");
      var imgW = imgObj.width;
      var imgH = imgObj.height;
      let canvas = document.createElement("canvas");
      canvas.width = imgW;
      canvas.height = imgH;
      console.log("canvas",canvas)
      let canvasContext = canvas.getContext("2d");
      canvasContext.drawImage(imgObj, 0, 0);
      var imgPixels = canvasContext.getImageData(0, 0, imgW+2, imgH+2);
      console.log("wh",imgPixels,imgPixels.height,imgPixels.width)
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
      canvasContext.putImageData(imgPixels, 0, 0, 0, 0, imgPixels.width, imgPixels.height);
      this.$refs.img.style.width = imgPixels.width + "px";
      this.$refs.img.style.height = imgPixels.height + "px";
      console.log(canvas.toDataURL());
      this.pasteImage = canvas.toDataURL();
    },
  },
};
</script>

<style scoped lang="less">
.page {
  text-align: left;
  section {
    margin: 20px;
  }
  img {
    border: 1px solid #ccc;
  }
}
</style>
