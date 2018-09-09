<!--  -->
<template>
  <swiper :options="swiperOption" ref="mySwiper">
    <!-- slides -->
    <swiper-slide v-for='item in recommends' :key="item.href">
      <img :src="item.picUrl" />
    </swiper-slide>
    <!-- Optional controls -->
    <div class="swiper-pagination"  slot="pagination"></div>
  </swiper>
</template>

<script>
// require styles
import 'swiper/dist/css/swiper.css' 
import axios from 'axios'
import { ERR_OK } from 'api/config'
import { swiper, swiperSlide } from 'vue-awesome-swiper'
export default {
  name: '',

  data () {
    return {
      swiperOption: {
        autoplay: true,
        loop: true,
        pagination: {
          el: '.swiper-pagination'
        }
      },
      recommends: []
    }
  },

  created(){
    this.getRecommend();
  },

  mounted(){},

  watch: {},

  computed: {},

  methods: {
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
    }
  },

  components: {
    swiper, 
    swiperSlide
  }
}

</script>
<style lang="stylus" scoped>
.swiper-slide
  img 
    width: 100%
</style>