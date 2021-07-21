<template>
  <div>
    <button @click="createArray">生成数组</button><br />
    {{ sortArray }}<br />
    <input type="number" v-model="k" />
    <button @click="toFindK(sortArray, 0, sortArray.length, k)">找第k大</button><br />
    {{result}}
  </div>
</template>

<script>
export default {
  name: "FindKNum",
  //部件
  components: {},
  //静态
  props: {},
  //对象内部的属性监听，也叫深度监听
  //请求数据
  created() {
    console.log(this.findK([9, 6, 32, 4, 8, 12, 5, 7], 0, 8, 3));
  },
  mounted() {},
  data() {
    return {
      sortArray: [],
      k: 0,
      result: "",
    };
  },
  watch: {},
  //属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
  computed: {},
  //方法表示一个具体的操作，主要书写业务逻辑；
  methods: {
    toFindK(array, left, right, k) {
      this.result = this.findK(array, left, right, k);
    },
    createArray() {
      this.sortArray = [];
      const c = 20;
      for (let i = 0; i < c; i++) {
        this.sortArray.push(Math.round(Math.random() * c));
      }
    },
    findK(array, left, right, k) {
      let i = this.partition(array, left, right);
      if (i == k - 1) {
        return array[k - 1];
      } else if (i > k - 1) {
        return this.findK(array, left, i - 1, k);
      } else if (i < k - 1) {
        return this.findK(array, i + 1, right, k);
      }
      return 0;
    },
    partition(arr, left, right) {
      // 分区操作
      var pivot = left, // 设定基准值（pivot）
        index = pivot + 1;

      for (var i = index; i <= right; i++) {
        if (arr[i] < arr[pivot]) {
          [arr[i], arr[index]] = [arr[index], arr[i]];
          index++;
        }
      }
      [arr[pivot], arr[index - 1]] = [arr[index - 1], arr[pivot]];
      return index - 1;
    },
  },
};
</script>

<style scoped></style>
