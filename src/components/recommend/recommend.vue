<!--  -->
<template>
  <div class="recommend">
    <Scroll class="recommend-content" :data="discList" ref="scroll">
      <div>
        <div v-if="recommends.length" class="slider-wrapper">
          <swiper :options="swiperOption" ref="mySwiper">
            <!-- slides -->
            <swiper-slide v-for='item in recommends' :key="item.href">
              <img :src="item.picUrl" @load="loadImage" />
            </swiper-slide>
            <!-- Optional controls -->
            <div class="swiper-pagination"  slot="pagination"></div>
          </swiper>
        </div>
        <div class="recommend-list">
          <h1 class="list-title">热门歌单推荐</h1>
          <ul>
            <li v-for="item in discList" class="item" :key="item.dissid">
              <div class="icon">
                <img width="60" height="60" v-lazy="item.imgurl" alt="">
              </div>
              <div class="text">
                <h2 class="name" v-html="item.creator.name"></h2>
                <p class="desc" v-html="item.dissname"></p>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div class="loading-container" v-show="!discList.length">
        <loading />
      </div>
    </Scroll>
  </div>
</template>

<script>
import Loading from 'base/loading/loading'
import Scroll from 'base/scroll/scroll'
// import Slider from 'base/slider/hslider'
import 'swiper/dist/css/swiper.css' 
import { swiper, swiperSlide } from 'vue-awesome-swiper'
import axios from 'axios'
import { ERR_OK } from 'api/config'
export default {
  name: '',
abstract: true,
  data () {
    return {
      swiperOption: {
        autoplay: true,
        loop: true,
        pagination: {
          el: '.swiper-pagination'
        }
      },
      recommends: [],
      discList: []
    };
  },

  created(){
    this.getRecommend();
    setTimeout(() => {
      this.getDiscList();
    }, 2000)    
  },

  mounted(){    
    
  },
  activated: function () {
     console.log('activated')
  },

  watch: {},

  computed: {},

  methods: {
    // 轮播图
    getRecommend() {
      axios({
        url: 'api/musichall/fcgi-bin/fcg_yqqhomepagerecommend.fcg',
        method: 'get'
      })
      .then(res => {
        console.log('res: ', res)
        console.log('res.data.code: ', res.data.code)
        if(res.data.code === ERR_OK) {
          console.log(res.data.data.slider)
          this.recommends = res.data.data.slider
        }
      })
    },
    getDiscList() {
      // /qzone/fcg-bin/fcg_ucc_getcdinfo_byids_cp.fcg
      axios({
        url: 'https://www.easy-mock.com/mock/5b8fab1d06b4621da8247b00/example/getDiscList',
        method: 'get'
      })
      .then(res => {
        console.log('res-getDiscList: ', res)
        console.log('res.data.data.list: ', res.data.data.list)
        this.discList = res.data.data.list;
      })
    },    
    loadImage() {
      if (!this.checkloaded) {
        this.checkloaded = true
        console.log('this.$refs: ', this.$refs)
        this.$refs.scroll.refresh()
      }
    },
  },

  components: {    
    swiper, 
    swiperSlide,
    // Slider,
    Scroll,
    Loading
  },
  destroyed() {
  }
}

</script>
<style lang="stylus" scoped>
@import "~common/stylus/variable"
  .swiper-slide
    img 
      width: 100%
  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    .recommend-content
      height: 100%
      overflow: hidden
      .slider-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            display: flex
            flex-direction: column
            justify-content: center
            flex: 1
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
      .loading-container
        position: absolute
        width: 100%
        top: 50%
        transform: translateY(-50%)
</style>