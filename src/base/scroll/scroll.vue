<!--  -->
<template>
  <div ref="wrapper">
    <slot></slot>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: '',

  props: {
    probeType: {
      type: Number,
      default: 1
    },
    click: {
      type: Boolean,
      default: true
    },
    data: {
      type: Array,
      default: null
    }
  },

  data () {
    return {
    };
  },

  created(){},

  mounted(){
    setTimeout(() => {
      this.initScroll()
    }, 20)
  },

  watch: {
    // 监听数据的变化，只要数据变化就要重新计算高度
    data() {
      setTimeout(() => {
        this.refresh()
      }, 20)
    }
  },

  computed: {},

  methods: {
    initScroll() {
      if (!this.$refs.wrapper){
        return
      }
      this.scroll = new BScroll(this.$refs.wrapper, {
        probeType: this.probeType,
        click: this.click
      })
    },
    enable() {
      this.scroll && this.scroll.enable()
    },
    disable() {
      this.scroll && this.scroll.disable()
    },
    // 刷新scroll，重新计算高度
    refresh() {
      this.scroll && this.scroll.refresh()
    }
  },

  components: {}
}

</script>
<style lang="stylus" scoped>
</style>