<template>
  <div class="wrapper">
    <swiper :options="swiperOption"  v-if="showSwiper">
      <!-- slides -->
      <swiper-slide v-for = 'item of list' :key = 'item.id'>
        <img class="swiper-img" :src="item.imgUrl">
      </swiper-slide>
      <!--显示分页-->
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>
  </div>
  <!--来自github-->

</template>

<script>
    export default {
        name: "HomeSwiper",
      props: {
        list: Array
      },
      data() {
          return {
           /* 轮播图的轮播表示点*/
            swiperOption: {
              pagination: '.swiper-pagination',
              /*支持循环轮播*/
              loop: true,
              autoplay: true,
              speed: 3000,
            }
          }
      },
      computed: {
          showSwiper() {
            return this.list.length
          }
      }
    }
</script>

<style lang="stylus" scoped>
  /*">>>" 样式穿透*/
  .wrapper >>> .swiper-pagination-bullet
    background #ffffff
  .wrapper
    overflow hidden
    width 100%
    height 0
    padding-bottom 26.7%
    background #eee
    .swiper-img
      width 100%
    .swiper-pagination
      margin-bottom .2rem
</style>
