<template>
  <view class="container">
    <view class="iconfont iconvideocamera"></view>
    <uni-badge text="1"></uni-badge>
    <uni-badge text="2" type="success"></uni-badge>
    <uni-badge text="3" type="primary" :inverted="true"></uni-badge>
  </view>
</template>

<script>
// 引入uni-ui组件
import { uniBadge } from "@dcloudio/uni-ui";
export default {
  components: {
    uniBadge
  },
  onLoad() {
    // http://157.122.54.189:9088/image/v3/homepage/vertical
    // 原生微信小程序api语法
    wx.request({
      url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
      success: (res)=> {
        console.log(res);
      }
    });

    // 2 uni-api语法
    uni.request({
      url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
    })
    .then(res=>{
      console.log(res);
    })
  }
};
</script>

<style>
</style>