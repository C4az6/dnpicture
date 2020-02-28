<script>
export default {
  onLaunch: function() {},
  onShow: function() {},
  onHide: function() {},
  methods: {
    noDataToast() {
      uni.showToast({
        title: "没有数据了",
        icon: "none"
      });
    }
  }
};
</script>

<style>
@import "./styles/iconfont.wxss";
@import "./styles/base.wxss";
</style>
