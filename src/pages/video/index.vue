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
      <view class="content">
        <view v-if="current<4">
          <video-main :urlObj="{url:items[current].url, params:items[current].params}"></video-main>
        </view>
        <view v-if="current===4">
          <video-category></video-category>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
import videoCategory from "./video-category";
import videoMain from "./video-main";
export default {
  components: {
    uniSegmentedControl,
    videoCategory,
    videoMain
  },
  data() {
    return {
      items: [
        { title: "推荐",url:'http://157.122.54.189:9088/videoimg/v1/videowp/featured', params:{limit:30, skip:0, order: 'hot'}},
        { title: "娱乐",url:'http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a', params:{limit:30, skip:0, order: 'new'}},
        { title: "最新",url:'http://157.122.54.189:9088/videoimg/v1/videowp/videowp', params:{limit:30, skip:0, order: 'new'}},
        { title: "热门",url:'http://157.122.54.189:9088/videoimg/v1/videowp/videowp', params:{limit:30, skip:0, order: 'hot'}},
        { title: "分类",url:'http://157.122.54.189:9088/videoimg/v1/videowp/category'}
      ],
      current: 0
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
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
  }
}
</style>