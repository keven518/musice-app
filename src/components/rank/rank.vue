<!--  -->
<template>
  <div>
    排行页面
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: '',

  data () {
    return {
    };
  },

  created(){
    axios({
      url: 'api/musichall/fcgi-bin/fcg_yqqhomepagerecommend.fcg',
      method: 'get'
    })
    .then(res => {
      console.log('res: ', res)
    })
  },

  mounted(){},

  watch: {},

  computed: {},

  methods: {},

  components: {}
}

</script>
<style lang="stylus" scoped>
</style>