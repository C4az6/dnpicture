<template>
  <view>
    <view class="category-wrap">
      <navigator
        class="category-item"
        v-for="item in category"
        :key="item.id"
        :url="`/pages/imgCategory/index?id=${item.id}`"
      >
        <image :src="item.cover" mode="aspectFill" />
        <text>{{item.name}}</text>
      </navigator>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      category: []
    };
  },
  mounted() {
    // 修改页面的标题
    uni.setNavigationBarTitle({
      title: "分类"
    });
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url: "http://157.122.54.189:9088/image/v1/vertical/category"
      }).then(data => {
      this.category = data.res.category;
      });
    }
  }
};
</script>

<style lang="scss">
.category-wrap {
  display: flex;
  flex-wrap: wrap;
  .category-item {
    width: 33.33%;
    position: relative;
    image {
      height: 280rpx;
      border: 5rpx solid #fff;
    }

    text {
      position: absolute;
      width: 100%;
      height: 50rpx;
      left: 0;
      bottom: 0;
      color: #ffffff;
      // c3 渐变效果
      background-image: linear-gradient(
        to right top,
        rgba(0, 0, 0, 0.2),
        rgba(0, 0, 0, 0)
      );
      font-size: 34rpx;
      padding-left: 10rpx;
    }
  }
}
</style>