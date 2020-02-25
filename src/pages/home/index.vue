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
  }
};
</script>

<style>
</style>