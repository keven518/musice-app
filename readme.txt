讲师：https://github.com/ustbhuangyi?page=1&tab=repositories
3-2路由配置+顶导组件开发
redirect  vue-router 的重定向-redirect
https://www.cnblogs.com/jinsuo/p/8287819.html

4-4 轮播组件实现（上）
mouted执行时，created的数据可能没有获取到
created 定时器，然后看看mouted有没有执行

4-10 SCROLL组件的抽象和应用（上）
1、确保页面dom都渲染结束
mounted(){
    setTimeout(() => {

    }, 20)
  },
2、在初始化new BScroll() 如果this.$refs.wrapper 为undefined的会报错
3、了解this.scroll && this.scroll.disable() 语法
4、<Scroll class="recommend-content"> 此时mounted中this.initScroll()在数据获取前先执行，所以滚不动
5、<img @load

总结：BScroll什么时候是需要重新计算（scroll.refresh）的。他的渲染原理是，根据初始化时期，或数据变更时
期计算父元素高度和子元素高度之差做计算，算他可以滚动的位置。实例化或调用refresh时一定要保证dom是渲
染好的能计算高度的。


5-3、歌手数据处理和inger类的封装
1、通过list.forEach ，把相同的key集合到一个对象里，做不同的属性
2、for(let key in map) 遍历每个属性
3、字母排序
4、hot.concat(ret) 合并两个数组