<template>
  <view>
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
    <view class="album-swiper">
      <swiper class="swiper" indicator-dots autoplay circular>
        <swiper-item v-for="item in bannerList" :key="item.id">
          <image :src="item.thumb"/>
        </swiper-item>
      </swiper>
    </view>
  </view>
</template>

<script>
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
      album: []
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
        this.bannerList = data.res.banner;
        this.album = data.res.album;
      });
    }
  }
};
</script>

<style lang="scss">
  .album-swiper {
    .swiper {
      height: calc(750rpx / 2.3);
      // height: 326rpx;
      image {
        height: 100%;
      }
    }
  }
</style>