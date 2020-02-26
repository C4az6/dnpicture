<template>
  <view>
    <view class="album-bg">
      <image :src="album.cover" mode="widthFix" />
      <view class="album-title">
        <view class="album-name">{{album.name}}</view>
        <view class="album-btn">关注专辑</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      // 上一个页面传递过来的id
      params: {
        limit: 30,
        order: "new",
        skip: 0,
        first: 1
      },
      id: -1,
      album: {},
      wallpaper: []
    };
  },
  methods: {
    getList() {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.params
      }).then(data => {
        this.album = data.res.album;
        this.wallpaper = data.res.wallpaper;
      });
    }
  },
  onLoad(options) {
    // this.id = options.id;
    this.id = "5e26b92be7bce739af7644b3";
    this.getList();
  }
};
</script>

<style lang="scss">
.album-bg {
  position: relative;
  image {
  }

  .album-title {
    display: flex;
    padding: 20rpx;
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    justify-content: space-between;
    align-items: center;
    .album-name {
      font-size: 34rpx;
      color: #fff;
    }

    .album-btn {
      font-size: 26rpx;
      background-color: $color;
      color: #fff;
      padding: 15rpx;
      border-radius: 10rpx;
    }
  }
}
</style>