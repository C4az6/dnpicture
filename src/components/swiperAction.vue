/* 
  1 slot
  2 对外提供数据 滑动的方向
 */

 <template>
  <view @touchstart="handleTouchstart" @touchend="handleTouchend">
    <slot></slot>
  </view>
</template>
 
 <script>
export default {
  data() {
    return {
      startTime: 0,
      endTime: 0,
      startX: 0,
      startY: 0,
      endX: 0,
      endY: 0,
      direction: ""
    };
  },
  methods: {
    handleTouchstart(event) {
      this.startTime = Date.now();
      this.startX = event.changedTouches[0].clientX;
      this.startY = event.changedTouches[0].clientY;
    },
    handleTouchend(event) {
      this.endTime = Date.now();
      this.endX = event.changedTouches[0].clientX;
      this.endY = event.changedTouches[0].clientY;
      if (this.endTime - this.startTime > 3000) {
        return;
      }

      if (Math.abs(this.endX - this.startX) > 10 && Math.abs(this.endY - this.startY) < 10) {
        this.direction = this.endX - this.startX > 0 ? "right" : "left";
      } else {
        return;
      }
      this.$emit("swiperAction", { direction: this.direction });
    }
  }
};
</script>
 
 <style>
</style>