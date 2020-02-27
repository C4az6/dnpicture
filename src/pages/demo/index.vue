<template>
  <view @touchstart="handleTouchstart" @touchend="handleTouchend">学习触屏事件</view>
</template>

<script>
/* 
  1.给容器绑定2个触屏事件 touchstart和touchend
  2.用户按下屏幕事件
    1 记录用户按下屏幕的时间 Date.now()时间戳返回1970-1-1到现在的毫秒数
    2 记录用户按下屏幕的坐标x和y
  3 用户离开屏幕事件
    1 记录用户离开屏幕的时间 Date.now()
    2 记录用户离开屏幕的坐标x和y
    3 根据2个时间运算 判断用户按下屏幕时长是否合法
    4 根据2对坐标 判断距离是否合法 判断 滑动方向

*/
export default {
  data() {
    return {
      startTime: "",
      endTime: "",
      startX: "",
      startY: "",
      endX: "",
      endY: ""
    };
  },
  methods: {
    handleTouchstart(event) {
      // 记录用户按下的时间
      this.startTime = Date.now();
      // 记录用户按下的X坐标
      this.startX = event.changedTouches[0].clientX;
      // 记录用户按下的Y坐标
      this.startY = event.changedTouches[0].clientY;
    },
    handleTouchend(event) {
      // 记录用户离开的时间
      this.endTime = Date.now();
      // 记录用户离开的X坐标
      this.endX = event.changedTouches[0].clientX;
      // 记录用户离开的Y坐标
      this.endY = event.changedTouches[0].clientY;

      // 根据2个时间运算且判断按下屏幕时长是否合法 如果差值大于3000毫秒视为非法 直接return
      if (this.endTime - this.startTime > 3000) {
        return;
      }
      // 根据2对坐标判断距离是否合法并且判断滑动方向
      let direction = "";
      let directionY = "";
      if (Math.abs(this.endX - this.startX) > 10) {
        // 合法的滑动
        direction = this.endX - this.startX > 0 ? "Right" : "Left";
        console.log(direction);
      } else {
        // return;
      }

      if (Math.abs(this.endY - this.startY) > 3) {
        // 合法的滑动
        directionY = this.endY - this.startY > 0 ? "BOTTOM" : "TOP";
        console.log(directionY);
      } else {
        
      }
    }
  }
};
</script>

<style lang="scss">
view {
  width: 100%;
  height: 500rpx;
  background-color: lightgreen;
}
</style>