<script>
export default {
  onLaunch: function() {
    console.log("App Launch");
  },
  onShow: function() {
    console.log("App Show");
  },
  onHide: function() {
    console.log("App Hide");
  },
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
