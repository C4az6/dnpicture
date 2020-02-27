<template>
  <view>
    <!-- 专辑背景 start -->
    <view class="album-bg">
        <image :src="album.cover" mode="widthFix" />
      <view class="album-title">
        <view class="album-name">{{album.name}}</view>
        <view class="album-btn">关注专辑</view>
      </view>
    </view>
    <!-- 专辑背景 end -->

    <!-- 专辑作者 start -->
    <view class="album-author">
      <view class="album-title">
        <image :src="album.user.avatar" mode="widthFix" />
        <view class="album-name">{{album.user.name}}</view>
      </view>
      <view class="album-desc">
        <text>{{album.desc}}</text>
      </view>
    </view>
    <!-- 专辑作者 end -->

    <!-- 专辑图片 start -->
    <view class="album-image">
      <view class="album-item" v-for="(item, index) in wallpaper" :key="item.id">
        <go-detail :list="wallpaper" :index="index">
          <image mode="aspectFill" :src="item.thumb+item.rule.replace('$<Height>',360)" />
        </go-detail>
      </view>
    </view>
    <!-- 专辑图片 end -->
  </view>
</template>

<script>
import goDetail from "@/components/goDetail";
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
      wallpaper: [],
      hasLimit: true
    };
  },
  methods: {
    getList() {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper`,
        data: this.params
      }).then(data => {
        if (Object.keys(this.album).length === 0) {
          this.album = data.res.album;
        }
        if (data.res.wallpaper.length === 0) {
          getApp().noDataToast();
          this.hasLimit = false;
          return;
        }
        this.wallpaper = [...this.wallpaper, ...data.res.wallpaper];
      });
    },
    handleSwiperAction(e){
      console.log(e);
    }
  },
  onLoad(options) {
    this.id = options.id;
    this.getList();
  },
  // 上拉触底事件,这个是生命周期函数,因此不能写在methods方法里面
  onReachBottom() {
    if (this.hasLimit) {
      this.params.first = 0;
      this.params.skip += this.params.limit;
      this.getList();
    } else {
      getApp().noDataToast();
    }
  },
  components: {
    goDetail,
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

.album-author {
  padding: 15rpx;
  .album-title {
    display: flex;
    image {
      width: 50rpx;
    }

    .album-name {
      font-size: 26rpx;
      color: #000;
      margin-left: 10rpx;
    }
  }

  .album-desc {
    margin-top: 5rpx;
    text {
      font-size: 24rpx;
      color: #666;
    }
  }
}

.album-image {
  display: flex;
  flex-wrap: wrap;
  .album-item {
    width: 33.33%;
    image {
      // width: 200rpx;
      height: 200rpx;
      border: 3rpx solid #fff;
    }
  }
}
</style>