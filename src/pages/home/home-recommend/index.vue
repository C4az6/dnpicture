<template>
  <view>
    <view class="recommend-wrap">
        <image :src="item.thumb" mode="widthFix" v-for="(item) in recommends" :key="item.id" />
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      // 推荐列表数据
      recommends: []
    };
  },
  mounted() {
    this.request({
      url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
      data: {
        // 要获取几条
        limit: 30,
        // 关键字
        order: "hot",
        // 要跳过几条
        skip: 0
      }
    }).then(res => {
      console.log(res.res);
      this.recommends = res.res.homepage[1].items;
    });
  }
};
</script>

<style lang="scss" scoped>
  .recommend-wrap {
    display: flex;
    flex-wrap: wrap;
    >image {
      width: 50%;
      border: 5rpx solid #fff;
    }
  }
</style>