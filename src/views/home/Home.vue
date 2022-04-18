<template>
  <div id="home">

      <nav-bar class="home-nav">
        <div slot="center">购物街</div>
      </nav-bar>
      <tab-control class="tab-control tab1" :titles="['流行','新款','精选']" @tabClick="tabClick" ref="TabControl1" v-show="isTabShow"/>
      <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll" :pull-up-load="true" @pullingUp="loadMore">
        <home-swiper :banner="banner" @swiperImagesLoad="swiperImagesLoad"></home-swiper>
        <recommend-view :recommend="recommend"/>
        <feature-view />
        <tab-control class="tab-control" :titles="['流行','新款','精选']" @tabClick="tabClick" ref="TabControl2"/>
        <goods-list :goods="showGoods" />
      </scroll>

      <back-top @click.native="backClick" v-show="isShow"/>
    <keep-alive>
    </keep-alive>
  </div>
</template>

<script>

  import HomeSwiper from "./childComps/HomeSwiper.vue"
  import RecommendView from "./childComps/RecommendView.vue"
  import FeatureView from "./childComps/FeatureView.vue"

  import NavBar from "components/common/navbar/NavBar"
  import TabControl from "components/content/tabControl/tabControl.vue"
  import GoodsList from "components/content/goods/GoodsList"

  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'
  import {getHomeMultidata,getHomeGoods} from "network/Home"

  export default {
    name: "Home",
    components:{
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodsList,
      Scroll,
      BackTop,
    },
    data(){
      return {
        banner:[],
        recommend:[],
        goods:{
          'pop' : { page : 0, list : [] },
          'new': { page : 0, list : [] },
          'sell': { page : 0, list : [] },
        },
        currentType:'pop',
        isShow:false,
        tabOffsetTop:0,
        isTabShow:false,
        saveY:0
      }
    },
    created(){
      this.getHomeMultidata();
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');
    },
    activated() {
      this.$refs.scroll.refresh()
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
      
    },
    deactivated() {
      this.saveY = this.$refs.scroll.scroll.y;
    },
    computed:{
      showGoods(){
        return this.goods[this.currentType].list
      }
    },
    mounted(){
      const refresh = this.debounce(this.$refs.scroll.refresh,200);
        this.$bus.$on('imagesLoad',()=>{
          refresh();
      })
    },
    methods:{
      // ******** 监听事件 ********
      tabClick(index){
        console.log(index);
        switch(index){
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
        this.$refs.TabControl1.currentIndex = index;
        this.$refs.TabControl2.currentIndex = index;
      },
      backClick(){
        this.$refs.scroll.scrollTo(0,0)
      },
      contentScroll(position){
        this.isShow =  (-position.y)>1000;
        
        this.isTabShow = (-position.y) > this.tabOffsetTop
      },
      loadMore(){
        console.log("上拉加载更多");
        this.getHomeGoods(this.currentType);
        this.$refs.scroll.scroll.refresh();
      },
      debounce(func, delay) {
        let timer = null
        return function (...args) {
          if (timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this, args)
          }, delay)
        }
      },
      swiperImagesLoad(){
          this.tabOffsetTop = this.$refs.TabControl2.$el.offsetTop;
      },

      

      // ********** 网络请求 ************
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          // this.result = res;
          this.banner = res.data.banner.list;
          this.recommend = res.data.recommend.list;
        })
      },
      getHomeGoods(type){
        getHomeGoods().then(res=>{
          const page = this.goods[type].page +1
          getHomeGoods(type,page).then(res =>{
            this.goods[type].list.push(...res.data.list)
            this.goods[type].page += 1
          })
      }) 
      }
    }
  }
</script>

<style scoped>
  #home {
    padding-top: 44px;
    height: 100vh;
    position: relative;
    overflow: auto;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }
  .tab1{position:fixed;}
  .tab-control {
    position: sticky;
    top: 0px;
    z-index: 9;
    background-color: #fff;
  }

  .content {
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

</style>
