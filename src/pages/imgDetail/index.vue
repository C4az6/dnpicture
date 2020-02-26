<template>
  <view>
    <!-- 用户信息部分 start -->
    <view class="userinfo-wrap">
      <view class="user-icon">
        <image :src="imgInfo.user.avatar" mode="widthFix" />
      </view>
      <view class="user-info">
        <view class="user-name">{{imgInfo.user.name}}</view>
        <view class="user-time">{{imgInfo.atime}}</view>
      </view>
    </view>
    <!-- 用户信息部分 end -->

    <!-- 图片部分 start -->
    <view class="img-bg">
      <image :src="imgInfo.thumb+imgInfo.rule.replace('$<Height>',360)" mode="widthFix" />
    </view>
    <!-- 图片部分 end -->

    <!-- 点赞部分 start -->
    <view class="user-collect">
      <text class="iconfont icondianzan">{{imgInfo.rank}}</text>
      <text class="iconfont iconshoucang">收藏</text>
    </view>
    <!-- 点赞部分 end -->
  </view>
</template>

<script>
export default {
  data() {
    return {
      imgInfo: {}
    };
  },
  onLoad() {
    console.log(getApp().globalData);
    const { imgList, imgIndex } = getApp().globalData;
    this.imgInfo = imgList[imgIndex];
    console.log(this.imgInfo);
  }
};
</script>

<style lang="scss">
.userinfo-wrap {
  display: flex;
  padding: 20rpx;
  .user-icon {
    padding: 0 10rpx;
    width: 100rpx;
    height: 100rpx;
    image {
      border-radius: 50%;
    }
  }

  .user-info {
    padding: 10rpx;
    .user-name {
      font-weight: 600;
      font-size: 28rpx;
      color: #000;
    }

    .user-time {
      color: #ccc;
      font-size: 24rpx;
    }
  }
}

.img-bg {
  image {

  }
}

.user-collect {
  display: flex;
  align-items: center;
  height: 80rpx;
  border-bottom: 3rpx solid #ccc;
  text.iconfont.icondianzan {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    
  }

  text.iconfont.iconshoucang {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}


</style>