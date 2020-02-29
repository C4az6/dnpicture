<template>
    <srcoll-view class="video-main" @scrolltolower="handleTolower">
      <view class="video-item" v-for="item in videoList" :key="item.id">
        <image :src="item.img" mode="widthFix" />
      </view>
    </srcoll-view>
</template>

<script>
export default {
  props: {
    urlObj: Object
  },
  data(){
    return {
      videoList: []
    }
  },
  mounted () {
    this.getList();
  },
  watch: {
    urlObj(){
      this.getList();
    }
  },
  methods: {
    getList(){
      this.request({
        url: this.urlObj.url,
        params: this.urlObj.params
      })
      .then(data=>{
        if(!data.res) {
        
          this.videoList = [
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"2132131"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213212"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213213"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213214"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213215"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213216"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"2132173"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213218"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213219"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"213210"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"21321123"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"21321333"},
            {img:'http://img5.adesk.com/5d35a2d9e7bce75e0214ceab?imageMogr2/thumbnail/!350x540r/gravity/Center/crop/350x540',id:"21321322"}
            ]
        }else {
          this.videoList = data.res.videowp;
        }
      })
    }
  }
}
</script>

<style lang="scss">
.video-main {
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36px);
  .video-item {
    width: 33.33%;
    image {
      border: 5rpx solid #fff;
    }
  }
}
</style>