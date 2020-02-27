<template>
  <view>
    <!-- 用户信息部分 start -->
    <view class="userinfo-wrap">
      <view class="user-icon">
        <image :src="imgInfo.user.avatar" mode="widthFix" />
      </view>
      <view class="user-info">
        <view class="user-name">{{imgInfo.user.name}}</view>
        <view class="user-time">{{imgInfo.cntime}}</view>
      </view>
    </view>
    <!-- 用户信息部分 end -->

    <!-- 图片部分 start -->
    <view class="img-bg">
      <image :src="imgInfo.thumb2" mode="widthFix" />
    </view>
    <!-- 图片部分 end -->

    <!-- 点赞部分 start -->
    <view class="user-collect">
      <text class="iconfont icondianzan">{{imgInfo.rank}}</text>
      <text class="iconfont iconshoucang">收藏</text>
    </view>
    <!-- 点赞部分 end -->

    <!-- 专辑信息 start -->
    <view class="album-wrap">
      <view class="album-title">相关</view>
      <view class="album-list">
        <view class="album-item" v-for="item in album" :key="item.id">
          <view class="album-image">
            <image :src="item.cover" mode="aspectFill"/>
          </view>
          <view class="album-info">
            <view class="album-text">专辑</view>
            <view class="album-name">{{item.name}}</view>
            <text class="iconfont iconiconfontjiantou4"></text>
          </view>
        </view>
      </view>
    </view>
    <!-- 专辑信息 end -->
  </view>
</template>

<script>
import moment from "moment";
// 设置语言为中文
moment.locale("zh-cn");
export default {
  data() {
    return {
      imgInfo: {},
      album: []
    };
  },
  onLoad() {
    const { imgList, imgIndex } = getApp().globalData;
    this.imgInfo = imgList[imgIndex];
    this.imgInfo.thumb2 =
      this.imgInfo.thumb + this.imgInfo.rule.replace("$<Height>", 360);
    this.imgInfo.cntime = moment(this.imgInfo.atime * 1000).fromNow();

    // 获取图片详情的id
    this.getComments(this.imgInfo.id);
  },
  methods: {
    getComments(id) {
      this.request({
        url: `http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/${id}/comment`
      }).then(data => {
        this.album = data.res.album;
        this.hot = data.res.hot;
        this.comment = data.res.comment;
        console.log(this.album);
        console.log(this.hot);
        console.log(this.comment);
      });
    }
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
  height: 80rpx;
  line-height: 80rpx;
  border-bottom: 3rpx solid #ccc;
  text.iconfont.icondianzan {
    flex: 1;
    display: flex;
    justify-content: center;
  }

  text.iconfont.iconshoucang {
    flex: 1;
    display: flex;
    justify-content: center;
  }
}

.album-wrap {
  padding: 10rpx;
  .album-title {
    padding: 10rpx 0;
  }

  .album-list {
    .album-item {
      display: flex;
      padding: 10rpx 0;
      border-bottom: 10rpx solid #eee;
      .album-image {
        flex: 1;
        image {
          width: 180rpx;
          height: 180rpx;
        }
      }

      .album-info {
        flex: 3;
        padding-left: 20rpx;
        position: relative;
        .album-text {
          color: #fff;
          background-color: $color;
          padding: 20rpx;
          font-size: 24rpx;
          width: 90rpx;
          height: 50rpx;
          display: flex;
          justify-content: center;
          align-items: center;
        }
        .iconfont {
          position: absolute;
          top: 50%;
          right: 10%;
          transform: translateY(-50%);
        }

        .album-name {
          color: #000;
        }
      }
    }
  }
}
</style>