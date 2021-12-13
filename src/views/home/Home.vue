<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners"/>
    <recommend-view :recommends="recommends"/>
    <feature-view />
    <tab-control :titles="['流行','时尚','新款']" class="tabControl" />
    
    <goods-list :goods="goods['pop'].list"/>
    <ul>
      <li>sad</li>
      <li>w</li>
      <li>b</li>
      <li>w</li>
      <li>n</li>
      <li>m</li>
      <li>k</li>
      <li>l</li>
       <li>sad</li>
      <li>w</li>
      <li>b</li>
      <li>w</li>
      <li>n</li>
      <li>m</li>
      <li>k</li>
      <li>l</li>
       <li>sad</li>
      <li>w</li>
      <li>b</li>
      <li>w</li>
      <li>n</li>
      <li>m</li>
      <li>k</li>
      <li>l</li>
       <li>sad</li>
      <li>w</li>
      <li>b</li>
      <li>w</li>
      <li>n</li>
      <li>m</li>
      <li>k</li>
      <li>l</li>
       <li>sad</li>
      <li>w</li>
      <li>b</li>
      <li>w</li>
      <li>n</li>
      <li>m</li>
      <li>k</li>
      <li>l</li>
       <li>sad</li>
      <li>w</li>
      <li>b</li>
      <li>w</li>
      <li>n</li>
      <li>m</li>
      <li>k</li>
      <li>l</li>
       <li>sad</li>
      <li>w</li>
      <li>b</li>
      <li>w</li>
      <li>n</li>
      <li>m</li>
      <li>k</li>
      <li>l</li>
    </ul>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar';
  import HomeSwiper from './childComps/HomeSwiper';
  import RecommendView from './childComps/RecommendView';
  import FeatureView from './childComps/FeatureView.vue';

  import TabControl from 'components/content/tabControl/TabControl';
  import GoodsList from 'components/content/goods/GoodsList'

  import {getHomeMultidata, getHomeGoods} from "network/home";

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
    data() {
      return {
        banners: [],
        recommends: [],
        goods:{
          'pop':{page:0, list:[]},
          'new':{page:0, list:[]},
          'sell':{page:0, list:[]},
        }
          
      }
    },
    created() {
      this.getHomeMultidata();
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');
    },
    methods:{
      // 1.请求多个数据
      getHomeMultidata(){
         getHomeMultidata().then(res => {
        // this.result = res;
        console.log(res)
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
      },
      //请求商品数据
      getHomeGoods(type){
        const page = this.goods[type].page + 1; 
        getHomeGoods(type,page).then(res =>{
         this.goods[type].list.push(...res.data.list);
         this.goods[type].page += 1;
        })
      }
    }
  }
</script>

<style scoped>
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;
    
    position:fixed;
    top:0px;
    left:0px;
    right:0px;
    z-index: 99999;
  }
  .tabControl{
    position:sticky;
    top:44px;
  }
</style>
