<!--  -->
<template>
  <div class="singer">
    <list-view :data="singers"></list-view>
  </div>
</template>

<script>
import axios from 'axios'
import { ERR_OK } from 'api/config'
import Singer from 'common/js/singer'
import listView from 'base/listview/listview'


const HOT_NAME = '热门'
const HOT_SINGER_LEN = 10

export default {
  name: '',

  data () {
    return {
      singers: []
    };
  },

  created(){
    this.getSingerList();
  },

  mounted(){},

  watch: {},

  computed: {},

  methods: {
    getSingerList(){
      axios({
        url: 'https://www.easy-mock.com/mock/5b8fab1d06b4621da8247b00/example/singer_list',
        method: 'get'
      })
      .then(res => {
        console.log('res: ', res)
        if(res.data.code === ERR_OK){
          this.singers = this.normalizeSinger(res.data.data.list);
          console.log('normalizeSinger: ', this.normalizeSinger(res.data.data.list))
        }
      })
    },
    normalizeSinger(list) {
      let map = {
        hot: {
          title: HOT_NAME,
          items: []
        }
      }
      list.forEach((item, index) => {
        if (index < HOT_SINGER_LEN) {
          map.hot.items.push(new Singer({
            id: item.Fsinger_mid,
            name: item.Fsinger_name,
          }))
        }

        const key = item.Findex
        if (!map[key]) {
          map[key] = {
            title: key,
            items: []
          }
        }

        map[key].items.push(new Singer({
          id: item.Fsinger_mid,
          name: item.Fsinger_name,
        }))
      })
      console.log('map: ', map)

      // 为了得到有序列表，我们需要处理 map
      let hot = []
      let ret = []
      for(let key in map) {
        let val = map[key]
        if(val.title.match(/[a-zA-Z]/)){
          ret.push(val)
        }else if(val.title === HOT_NAME){
          hot.push(val)
        }
      }
      ret.sort((a, b) => {
        return a.title.charCodeAt(0) - b.title.charCodeAt(0)
      })
      return hot.concat(ret);
    }
  },

  components: {
    listView
  }
}

</script>
<style lang="stylus" scoped>
.singer
    position: fixed
    top: 88px
    bottom: 0
    width: 100%
</style>