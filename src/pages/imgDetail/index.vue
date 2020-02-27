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
      <swiper-action @swiperAction="handleSwiperAction">
        <image :src="imgInfo.thumb" mode="widthFix" />
      </swiper-action>
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
            <image :src="item.cover" mode="aspectFill" />
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

    <!-- 最热评论 start -->
    <view class="comment-wrap hot" v-if="hot.length">
      <!-- 标题 -->
      <view class="comment-title">
        <text class="iconfont iconhot1"></text>
        <text class="title-text">最热评论</text>
      </view>
      <!-- 评论内容 -->
      <view class="comment-list">
        <view class="comment-item" v-for="item in hot" :key="item.id">
          <view class="user-info">
            <view class="user-avatar">
              <image :src="item.user.avatar" mode="widthFix" />
            </view>
            <view class="user-name">
              <view class="user-nikename">{{item.user.name}}</view>
              <view class="user-time">{{item.cnTime}}</view>
            </view>
          </view>
          <view class="comment-conent">
            <text class="comment-text">{{item.content}}</text>
            <text class="iconfont icondianzan">{{item.size}}</text>
          </view>
        </view>
      </view>
    </view>
    <!-- 最热评论 end -->

    <!-- 最新评论 start -->
    <view class="comment-wrap new" v-if="comment.length">
      <!-- 标题 -->
      <view class="comment-title">
        <text class="iconfont iconpinglun"></text>
        <text class="title-text">最新评论</text>
      </view>
      <!-- 评论内容 -->
      <view class="comment-list">
        <view class="comment-item" v-for="item in comment" :key="item.id">
          <view class="user-info">
            <view class="user-avatar">
              <image :src="item.user.avatar" mode="widthFix" />
            </view>
            <view class="user-name">
              <view class="user-nikename">{{item.user.name}}</view>
              <view class="user-time">{{item.cnTime}}</view>
            </view>
          </view>
          <view class="comment-conent">
            <text class="comment-text">{{item.content}}</text>
            <text class="iconfont icondianzan">{{item.size}}</text>
          </view>
        </view>
      </view>
    </view>
    <!-- 最热评论 end -->
  </view>
</template>

<script>
import moment from "moment";
import swiperAction from "@/components/swiperAction";
// 设置语言为中文
moment.locale("zh-cn");
export default {
  components: {
    swiperAction
  },
  data() {
    return {
      imgInfo: {},
      album: [],
      hot: [],
      comment: []
    };
  },
  onLoad() {
    const { imgList, imgIndex } = getApp().globalData;
    this.imgInfo = imgList[imgIndex];
    // this.imgInfo.thumb2 = this.imgInfo.thumb + this.imgInfo.rule.replace("$<Height>", 360);
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

        data.res.hot.forEach(
          v => (v.cnTime = moment(v.atime * 1000).fromNow())
        );
        data.res.comment.forEach(
          v => (v.cnTime = moment(v.atime * 1000).fromNow())
        );
      });
    },
    handleSwiperAction(e){
      console.log(e);
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
    border-bottom: 1rpx solid #eee;
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

/* 最热评论 */
.comment-wrap {
  padding: 10rpx;
  .comment-title {
    text.iconfont.iconhot1 {
      font-size: 40rpx;
      color: #ff1200;
    }
    text.title-text {
      margin-left: 10rpx;
      font-weight: 600;
      color: #000;
    }
  }

  .comment-list {
    .comment-item {
      padding-top: 20px;
      border-bottom: 15rpx solid #eee;
      .user-info {
        display: flex;
        .user-avatar {
          width: 15%;
          image {
          }
        }

        .user-name {
          flex: 1;
          margin-left: 20rpx;
          .user-nikename {
            color: #666;
          }

          .user-time {
            color: #ccc;
            font-size: 24rpx;
          }
        }
      }

      .comment-conent {
        display: flex;
        justify-content: space-between;
        margin-left: 15%;
        padding: 20rpx;
        text.comment-text {
          color: #000;
        }

        text.iconfont.icondianzan {
          font-size: 38rpx;
        }
      }
    }
  }
}

/* 最热评论 */

.comment-wrap.new {
  .iconfont {
    color: #92bdd8;
    font-size: 40rpx;
  }
}
</style>