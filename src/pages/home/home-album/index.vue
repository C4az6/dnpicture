<template>
  <scroll-view class="scroll" scroll-y @scrolltolower="handleLower">
    <!-- 微信小程序轮播图知识
    1 自动轮播  autoplay
    2 指示器    indicator-dots
    3 衔接轮播  circular
    4 swiper标签 默认的高度是150px
    5 image标签默认的宽度320px => 基本样式中重置了100%
      默认的高度240px
    6 要计算图片的宽度和高度的比例  图片的宽度/高度
    7 把图片的比例也写到swiper标签样式中去
    8 swiper-item 默认宽高是继承父元素的100%
    -->
    <!-- 轮播图 start -->
    <view class="album-swiper">
      <swiper class="swiper" indicator-dots autoplay circular>
        <swiper-item v-for="item in bannerList" :key="item.id">
          <image :src="item.thumb" />
        </swiper-item>
      </swiper>
    </view>
    <!-- 轮播图 end -->

    <!-- 列表 start -->
    <view class="album-list">
      <navigator
        class="album-item"
        v-for="item in album"
        :key="item.id"
        :url="`/pages/album/index?id=${item.id}`"
      >
        <view class="item-image">
          <image :src="item.cover" mode="aspectFill" />
        </view>
        <view class="item.info">
          <view class="item-name">{{item.name}}</view>
          <view class="item-desc ellipsis">{{item.desc}}</view>
          <view class="item-attention">
            <view class="btn">+ 关注</view>
          </view>
        </view>
      </navigator>
    </view>
    <!-- 列表 end -->
  </scroll-view>
</template>

<script>
import goDetail from "@/components/goDetail";
export default {
  data() {
    return {
      // 发送请求的参数
      params: {
        limit: 30,
        order: "new",
        skip: 0
      },
      // 轮播图数组
      bannerList: [],
      // 专辑数组
      album: [],
      hasLimit: true
    };
  },
  mounted() {
    // 修改页面的标题
    uni.setNavigationBarTitle({
      title: "专辑"
    });
    this.getList();
  },
  methods: {
    getList() {
      this.request({
        url: "http://157.122.54.189:9088/image/v1/wallpaper/album",
        data: this.params
      }).then(data => {
        if (this.bannerList.length === 0) {
          this.bannerList = data.res.banner;
        }
        if (data.res.album.length === 0) {
          getApp().noDataToast();
          this.hasLimit = false;
          return;
        }
        this.album = [...this.album, ...data.res.album];
      });
    },
    handleLower(e) {
      if (this.hasLimit) {
        this.params.skip += this.params.limit;
        this.getList();
      } else {
        getApp().noDataToast();
      }
    }
  },
  components: {
    goDetail
  }
};
</script>

<style lang="scss">
.scroll {
  height: calc(100vh - 36px);
}

.album-swiper {
  .swiper {
    height: calc(750rpx / 2.3);
    // height: 326rpx;
    image {
      height: 100%;
    }
  }
}

.album-list {
  padding: 10rpx;
  .album-item {
    display: flex;
    padding: 10rpx;
    border-bottom: 1rpx solid #ccc;
    .item-image {
      flex: 1;
      image {
        width: 200rpx;
        height: 200rpx;
      }
    }

    .item.info {
      flex: 2;
      padding: 10rpx;
      overflow: hidden;
      .item-name {
        font-size: 28rpx;
        color: #000;
      }

      .item-desc {
        font-size: 24rpx;
        color: #666;
        padding: 10rpx 0;
      }

      .item-attention {
        display: flex;
        justify-content: flex-end;
        padding: 10rpx 0;
        .btn {
          border: 2rpx solid $color;
          padding: 5rpx;
          font-size: 26rpx;
          color: $color;
        }
      }
    }
  }
}
</style>