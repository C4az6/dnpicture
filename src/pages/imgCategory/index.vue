<template>
  <view class="home-tab">
    <!-- 分段器 -->
    <view>
      <view class="title">
        <view class="inner-title">
          <uni-segmented-control
            :current="current"
            :values="items.map(v=>v.title)"
            @clickItem="onClickItem"
            style-type="text"
            active-color="#d21974"
          ></uni-segmented-control>

          <view class="iconfont iconsearch"></view>
        </view>
      </view>
      <scroll-view enable-flex="true" scroll-y @scrolltolower="handleTolower" class="content">
        <view class="cate-item" v-for="(item,index) in vertical" :key="item.id">
          <go-detail :list="vertical" :index="index">
            <image :src="item.thumb" mode="widthFix" />
          </go-detail>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import goDetail from "@/components/goDetail";
export default {
  components: {
    uniSegmentedControl,
    goDetail
  },
  data() {
    return {
      items: [
        { title: "最新", order: "new" },
        { title: "热门", order: "hot" }
      ],
      current: 0,
      vertical: [],
      id: 0,
      params: {
        limit: 30,
        skip: 0,
        order: "new"
      },
      // 判断是否还有分页数据
      hasLimit: true
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
        this.params.skip = 0;
        this.vertical = [];
        this.params.order = this.items[this.current].order;
        this.getList(this.id);
      }
    },
    handleTolower() {
      // 触底之后发送请求加载分页数据
      if (this.hasLimit) {
        this.getList(this.id);
      } else {
        getApp().noDataToast();
        return;
      }
    },
    getList(id) {
      this.request({
        url: `http://157.122.54.189:9088/image/v1/vertical/category/${id}/vertical`,
        data: this.params
      }).then(data => {
        if (data.res.vertical.length === 0) {
          this.hasLimit = false;
          getApp().noDataToast();
          return;
        }
        this.vertical = [...this.vertical, ...data.res.vertical];
        this.params.skip += this.params.limit;
      });
    }
  },
  onLoad(options) {
    this.id = options.id;
    this.getList(this.id);
  }
};
</script>

<style lang="scss">
.home-tab {
  .title {
    position: relative;
    .inner-title {
      width: 60%;
      margin: 0 auto;
    }
    .iconfont {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 5%;
    }
  }
  .content {
    display: flex;
    flex-wrap: wrap;
    height: calc(100vh - 36px);
    .cate-item {
      width: 33.33%;
      image {
        border: 5rpx solid #fff;
      }
    }
  }
}
</style>
