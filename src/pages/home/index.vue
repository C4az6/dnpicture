<template>
  <view class="container">
    <home-album></home-album>
    <home-category></home-category>
    <home-new></home-new>
    <home-recommend></home-recommend>
  </view>
</template>

<script>
import homeAlbum from "./home-album";
import homeCategory from "./home-category";
import homeNew from "./home-new";
import homeRecommend from "./home-recommend";
export default {
  components: {
    homeAlbum,
    homeCategory,
    homeNew,
    homeRecommend
  }
};
</script>

<style>
</style>