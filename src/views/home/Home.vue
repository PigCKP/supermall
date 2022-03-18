<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <scroll class="content" ref="scroll" 
    :probe-type="3" 
    @scroll="contentClick" 
    :pull-up-load="true"
    @pullingUp="loadMore"
    >
      <home-swiper :banners="banners"/>
      <recommend-view :recommends="recommends"/>
      <feature-view />
      <tab-control :titles="['流行','时尚','新款']" class="tabControl" @tabClicks="tabClicks"/>
      <goods-list :goods="showGods"/>
    </scroll>

    <!-- 组件不能直接绑定点击事件,必须加native修饰符 -->
    <back-top @click.native="backClick" v-show="isShow" />
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar';
  import HomeSwiper from './childComps/HomeSwiper';
  import RecommendView from './childComps/RecommendView';
  import FeatureView from './childComps/FeatureView.vue';
  import Scroll from 'components/common/scroll/scroll'


  import TabControl from 'components/content/tabControl/TabControl';
  import GoodsList from 'components/content/goods/GoodsList';
  import BackTop from 'components/content/backTop/backTop'

  import {getHomeMultidata, getHomeGoods} from "network/home";


  export default {
    name: "Home",
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods:{
          'pop':{page:0, list:[]},
          'new':{page:0, list:[]},
          'sell':{page:0, list:[]},
        },
        currentType: 'pop',

        isShow:false,
      
          
      }
    },
    created() {

      this.getHomeMultidata();


      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');


    
    
    },
    mounted(){
      const refresh = this.debounce(this.$refs.scroll.refresh)
        //加载item图片加载完成
      this.$bus.$on('itemImageLoad',() =>  {
        refresh()
      })

      //对于refresh非常频繁的问题，进行防抖函数debounce

    },

    computed:{
      showGods(){
        return this.goods[this.currentType].list
      }
    },
    methods:{
      tabClicks(index){
        switch(index){
          case 0:
            this.currentType = 'pop';
            break;
          case 1:
            this.currentType = 'new';
            break;
          case 2:
            this.currentType = 'sell';
            break;

        }
      },

      debounce(func,delay){
        let timer = null
        return function (...args){
          if(timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this,args)
          }, delay);
        }
      },

      
      // 1.请求多个数据
      getHomeMultidata(){
         getHomeMultidata().then(res => {
        // this.result = res;
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


        //请求一次后，无法再次上拉加载更多
         this.$refs.scroll.finishPullUp()
        })
      },

      //回到顶部 
      backClick(){
        this.$refs.scroll.scrollTo(0,0,500);
      },

      contentClick(position){
        // console.log(position)
        this.isShow = -position.y > 1000
      },
      loadMore() {
        console.log('撒大声地');
        this.getHomeGoods(this.currentType);
        this.$refs.scroll.scroll.refresh()
      }
     
    }
  }
</script>

<style scoped>
  #home{
    height:100vh;
    position:relative;
  }
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;
    
    position:fixed;
    top:0px;
    left:0px;
    right:0px;
    z-index: 9;
  }
  .tabControl{
    position:sticky;
    top:44px;
    z-index:9;
  }
  /* .content{
    overflow:hidden;
    position:absolute;
    top:44px;
    bottom:49px;
    left:0;
    right:0;
  } */
  .content{
    height: calc(100% - 93px);
    overflow: hidden;
    margin-top:44px;
  }
</style>
