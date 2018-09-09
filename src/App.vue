<template>
  <div id="app">
    <m-header></m-header>    
    <tab/>
    <keep-alive>
      <router-view/>
    </keep-alive>
    
  </div>
</template>

<script>
import MHeader from 'components/m-header/m-header'
import Tab from 'components/tab/tab'
export default {
  name: 'App',
  components: {
    MHeader,
    Tab
  }
}
</script>

<style>
</style>
<style lang="stylus" rel="stylesheet/stylus">
</style>

