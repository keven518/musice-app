<!--  -->
<template>
  <Scroll class="listview" 
          :data="data" 
          ref="listview" 
          :listenScroll="listenScroll"
          :probeType = "probeType"
          @scroll="scroll"
  >
    <ul>
      <li v-for="(group, i) in data" class="list-group" :key="i" ref="listGroup">
        <h2 class="list-group-title">{{group.title}}</h2>
        <ul>
          <li v-for="item in group.items" class="list-group-item" :key="item.id">
            <img class="avatar" v-lazy="item.avatar" alt="">
            <span class="name">{{item.name}}</span>
          </li>
        </ul>
      </li>
    </ul>
    <div class="list-shortcut" @touchstart="onShortcutTouchStart" @touchmove.stop.prevent="onShortcutTouchMove">
      <ul>
        <li v-for="(item, i) in shortcutList" 
            :key="i" 
            :class="{'current': currentIndex===i}"
            :data-index="i"
        >
          {{item}}
        </li>
      </ul>
    </div>
  </Scroll>
</template>

<script>
import Scroll from 'base/scroll/scroll'
import { getDomData } from 'common/js/dom'

const ANCHOR_HEIGHT = 18

export default {
  name: '',

  props: {
    data: {
      type: Array,
      default: []
    }
  },

  data () {
    return {
      scrollY: -1,  // 滚动的位置
      currentIndex: 0

    };
  },

  created(){
    this.touch = {}
    this.listenScroll = true
    this.listHeight = []
    this.probeType = 3
  },

  mounted(){},

  watch: {
    data() {
      setTimeout(() => {
        this._calculateHeight()
        console.log('this.listHeight: ', this.listHeight)
      }, 20)
    },
    scrollY(newY) {
      // console.log('newY: ', newY)
      const listHeight = this.listHeight
      console.log('listHeight: ', listHeight)
      
      // 在中间部分滚动
      for(let i=0; i<listHeight.length; i++){
        let height1 = listHeight[i]
        let height2 = listHeight[i + 1]
        if(!height2 || (-newY > height1 && -newY < height2)) {
          this.currentIndex = i
          console.log('this.currentIndex: ', this.currentIndex)
          return
        }
      }

      this.currentIndex = 0
      
      
    }
  },

  computed: {
    shortcutList() {
      console.log('this.data: ', this.data)
      return this.data.map((group) => {
        return group.title.substr(0, 1)
      })
    }
  },

  methods: {
    onShortcutTouchStart(e) {
      console.log('e: ', e)
      let anchorIndex = getDomData(e.target, 'index')
      let firstTouch = e.touches[0]
      this.touch.y1 = firstTouch.pageY
      this.touch.anchorIndex = anchorIndex
      console.log('anchorIndex: ', anchorIndex)
      console.log('this.$refs.listGroup: ', this.$refs.listGroup)
      this._scrollTo(anchorIndex)
    },
    onShortcutTouchMove(e) {
      console.log('e: ', e)
      let firstTouch = e.touches[0]
      this.touch.y2 = firstTouch.pageY
      let delta = (this.touch.y2 - this.touch.y1) / ANCHOR_HEIGHT | 0
      console.log('delta: ', delta);
      let anchorIndex = this.touch.anchorIndex*1 + delta
      console.log('this.touch.anchorIndex: ', this.touch.anchorIndex)
      this._scrollTo(anchorIndex)
    },
    scroll(pos){
      console.log('pos: ', pos)
      this.scrollY = pos.y
    },
    _scrollTo(i) {
      this.$refs.listview.scrollToElement(this.$refs.listGroup[i], 0)
    },
    //计算高度
    _calculateHeight() {
      console.log('setTimeOut')
      this.listHeight = []
      const list = this.$refs.listGroup
      let height = 0
      this.listHeight.push(height)
      for(let i=0; i<list.length; i++){
        let item = list[i]
        height += item.clientHeight
        this.listHeight.push(height);
      }
    }
  },

  components: {
    Scroll
  }
}

</script>
<style lang="stylus" scoped>
@import "~common/stylus/variable"

  .listview
    position: relative
    width: 100%
    height: 100%
    overflow: hidden
    background: $color-background
    .list-group
      padding-bottom: 30px
      .list-group-title
        height: 30px
        line-height: 30px
        padding-left: 20px
        font-size: $font-size-small
        color: $color-text-l
        background: $color-highlight-background
      .list-group-item
        display: flex
        align-items: center
        padding: 20px 0 0 30px
        .avatar
          width: 50px
          height: 50px
          border-radius: 50%
        .name
          margin-left: 20px
          color: $color-text-l
          font-size: $font-size-medium
    .list-shortcut
      position: absolute
      z-index: 30
      right: 0
      top: 50%
      transform: translateY(-50%)
      width: 20px
      padding: 20px 0
      border-radius: 10px
      text-align: center
      background: $color-background-d
      font-family: Helvetica
      .item
        padding: 3px
        line-height: 1
        color: $color-text-l
        font-size: $font-size-small
        &.current
          color: $color-theme
    .list-fixed
      position: absolute
      top: 0
      left: 0
      width: 100%
      .fixed-title
        height: 30px
        line-height: 30px
        padding-left: 20px
        font-size: $font-size-small
        color: $color-text-l
        background: $color-highlight-background
    .loading-container
      position: absolute
      width: 100%
      top: 50%
      transform: translateY(-50%)
</style>