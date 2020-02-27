<template>
  <view
  @touchstart="handleTouchstart"
  @touchend="handleTouchend"
  >学习触屏事件</view>
</template>

<script>
export default {
  methods: {
    handleTouchstart(event){
      console.log("按下：",event.changedTouches[0].clientX);
      console.log("按下：",event.changedTouches[0].clientY);
    },
    handleTouchend(event){
      console.log("离开：",event.changedTouches[0].clientY);
      console.log("离开：",event.changedTouches[0].clientY);
    }
  }
  
}
</script>

<style lang="scss">
  view {
    width: 100%;
    height: 500rpx;
    background-color: lightgreen;
  }
</style>