<template>
    <view class="category-tab">
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
        123
      </view>
    </view>
  </view>
</template>

<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui";
export default {
  components: {
    uniSegmentedControl
  },
  data() {
    return {
      items: [
        { title: "最新" },
        { title: "热门" },
      ],
      current: 1
    };
  },
  methods: {
    onClickItem(e) {
      if (this.current !== e.currentIndex) {
        this.current = e.currentIndex;
      }
    }
  },
};
</script>

<style lang="scss">
.category-tab {
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