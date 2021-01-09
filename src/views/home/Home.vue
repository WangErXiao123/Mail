<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view/>
    <tab-control :titles="['流行','新款','精选']"/>
    <goods-list :goods="goods['pop'].list"/>
  </div>
</template>

<script>

import HomeSwiper from "@/views/home/childComps/HomeSwiper"
import RecommendView from "@/views/home/childComps/RecommendView"
import FeatureView from "@/views/home/childComps/FeatureView"

import NavBar from '@/components/common/navbar/NavBar'
import TabControl from "@/components/content/tabControl/TabControl"
import GoodsList from "@/components/content/goods/GoodsList";

import { getHomeMultidata,getHomeGoods } from "@/network/home"


export default {
name: "Home",
  components: {
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodsList
  },
  data(){
  return{
    banners:[],
    recommends:[],
    goods:{
      'pop':{page:0,list:[]},
      'new':{page:0,list:[]},
      'sell':{page:0,list:[]}
    }
  }
  },
  created() {
    this.getHomeMultidata()
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  methods:{
    getHomeMultidata(){
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
      })
    },
    getHomeGoods(type){
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res=>{
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
      })
    }
  }
}

</script>

<style scoped>
#home {
  height: 100vh;
  position: relative;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
</style>
