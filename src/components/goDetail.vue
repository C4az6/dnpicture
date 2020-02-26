<template>
  <view @click="handleClick">
    <slot></slot>
  </view>
</template>

<script>
export default {
  props: {
    list: Array,
    index: Number
  },
  mounted() {},
  methods: {
    handleClick() {
      // 1 将数据缓存下来 使用getApp()全局缓存方式
      // 2 实现点击跳转页面
      getApp().globalData.imgList = this.list;
      getApp().globalData.imgIndex = this.index;
      uni.navigateTo({
        url: "/pages/imgDetail/index"
      });
    }
  }
};
</script>

<style>
</style>