<!--  -->
<template>
  <div>
    请输入浮点数<input v-model="myNumber" type="number" /><br />
    请输入保留小数位数<input v-model="restLength" type="number" /><br />
    <button @click="numTo64">转为64进制</button>
    {{showNum}}
  </div>
</template>

<script>
const SIGNS = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ-+".split(
  ""
);
export default {
  name: "To64Tranform",
  data() {
    return {
        myNumber:null,
        restLength:5,
        showNum:""
    };
  },

  components: {},

  computed: {},

  mounted(){},

  methods: {
    numTo64(){
      let temInt = Math.floor(this.myNumber);
      let temDecical = this.myNumber - Math.floor(this.myNumber);
      this.showNum=this.intTo64(temInt) +(this.restLength==0?"":".")+ this.decimalTo64(temDecical);
    },
    decimalTo64(num){
      let result = [];
      for(let i = 1;i<=this.restLength;i++){
          
          if(num==0){
              result.push("0");
              continue;
          }
          num = num*64;
          result.push(SIGNS[Math.floor(num)]);
          num = num - Math.floor(num);
      }
      return result.join("");
    },
    intTo64(num) {
      let result = [];
      let div = num;
      while (div > 63) {
        result.push(SIGNS[div % 64]);
        div = Math.floor(div / 64);
      }
      result.push(SIGNS[div]);
      return result.reverse().join("");
    },
  },
};
</script>
<style lang="less" scoped></style>
