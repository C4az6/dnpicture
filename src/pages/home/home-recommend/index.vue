<template>
  <scroll-view
    class="recommend-view"
    @scrolltolower="handleToLower"
    scroll-y
    v-if="recommends.length>0"
  >
    <!-- 推荐start -->
    <view class="recommend-wrap">
      <navigator
        class="recommend-item"
        v-for="item in recommends"
        :key="item.id"
        :url="`/pages/album/index?id=${item.target}`"
      >
        <image :src="item.thumb" mode="widthFix" />
      </navigator>
    </view>
    <!-- 推荐end -->

    <!-- 月份 start -->
    <view class="months-wrap">
      <view class="months-title">
        <view class="months-title-info">
          <view class="months-info">
            <text>{{months.MM}} /</text>
            {{months.DD}} 月
          </view>
          <view class="months-text">{{months.title}}</view>
        </view>
        <view class="months-title-more">更多 ></view>
      </view>
      <view class="months-content">
        <view class="months-item" v-for="(item, index) in months.items" :key="item.id">
          <go-detail :list="months.items" :index="index">
            <image :src="item.thumb + item.rule.replace('$<Height>', 360)" mode="aspectFill" />
          </go-detail>
        </view>
      </view>
    </view>
    <!-- 月份 end -->

    <!-- 热门 start -->
    <view class="hot-wrap">
      <view class="hot-title">
        <text>热门</text>
      </view>
      <view class="hot-content">
        <view class="hot-item" v-for="(item,index) in hots" :key="item.id">
          <go-detail :list="hots" :index="index">
            <image :src="item.thumb" mode="widthFix" />
          </go-detail>
        </view>
      </view>
    </view>
    <!-- 热门 end -->
  </scroll-view>
</template>

<script>
// 引入moment库
import moment from "moment";
import goDetail from "@/components/goDetail";
export default {
  components: {
    goDetail
  },
  data() {
    return {
      // 推荐列表数据
      recommends: [],
      // 月份模块
      months: {},
      // 热门
      hots: [],
      // 请求的参数
      params: {
        // 要获取几条
        limit: 30,
        // 关键字
        order: "hot",
        // 要跳过几条
        skip: 0
      },
      // 是否还有分页数据
      hasLimit: true
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    // 获取列表数据
    getList() {
      this.request({
        url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
        data: this.params
      }).then(data => {
        // 判断是否还有数据
        if (data.res.vertical.length === 0) {
          // 没有分页数据了
          getApp().noDataToast();
          this.hasLimit = false;
          return;
        }
        // 第一次请求数据
        if (this.recommends.length === 0) {
          this.recommends = data.res.homepage[1].items;
          this.months = data.res.homepage[2];
          // 将时间戳改成 18号 / 月
          this.months.MM = moment(this.months.stime).format("MM");
          this.months.DD = moment(this.months.stime).format("DD");
        }

        // 获取热门数据列表
        this.hots = [...this.hots, ...data.res.vertical];
      });
    },
    handleToLower(e) {
      /* 
      1 修改参数 skip+=limit
      2 重新调函数发送请求
      3 请求回来成功 hots 数据叠加,使用拓展运算符...
      */
      if (this.hasLimit) {
        this.params.skip += this.params.limit;
        this.getList();
      } else {
        getApp().noDataToast();
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.recommend-view {
  // height: 屏幕的高度 - 头部标题的高度
  height: calc(100vh - 36px);
}
/* 推荐 start */
.recommend-wrap {
  display: flex;
  flex-wrap: wrap;
  .recommend-item {
    width: 50%;
    border: 5rpx solid #fff;
  }
}
/* 推荐 end */

/* 月份 start */
.months-wrap {
  .months-title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20rpx;
    .months-title-info {
      display: flex;
      color: $color;
      font-size: 30rpx;
      font-weight: 600;
      .months-info {
        text {
          font-size: 36rpx;
        }
      }

      .months-text {
        padding-left: 30rpx;
        font-size: 34rpx;
        color: #666;
      }
    }

    .months-title-more {
      color: $color;
      font-size: 26rpx;
    }
  }

  .months-content {
    display: flex;
    flex-wrap: wrap;
    .months-item {
      width: 33.33%;
      border: 5rpx solid #fff;
    }
  }
}
/* 月份 end */

/* 热门 start */
.hot-wrap {
  .hot-title {
    padding: 20rpx;
    text {
      padding-left: 20rpx;
      color: $color;
      font-size: 28rpx;
      border-left: 10rpx solid $color;
      font-weight: 600;
    }
  }
  .hot-content {
    display: flex;
    flex-wrap: wrap;
    .hot-item {
      width: 33.33%;
      border: 5rpx solid #fff;
      image {
      }
    }
  }
}
/* 热门 end */
</style>