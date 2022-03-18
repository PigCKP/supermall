<template>
  <div id="detail">
  <!-- <h2>{{iid}}</h2> -->
    <!-- 导航栏 -->
    <detail-nav-bar></detail-nav-bar>
    <detail-swiper :top-images="topImages"></detail-swiper>
    <detail-base-info :goods="goods"></detail-base-info>
    <detail-shop-info :shop="shop"></detail-shop-info>
  </div>
</template>

<script>
  import DetailNavBar from 'views/detail/childComps/DetailNavBar.vue'
  import {getDeatil,Goods,Shop} from '../../network/deatail'
  import DetailSwiper from 'views/detail/childComps/DetailSwiper.vue'
  import DetailBaseInfo from 'views/detail/childComps/DetailBaseInfo.vue'
  import DetailShopInfo from 'views/detail/childComps/DetailShopInfo.vue'
  export default {
    name: "Detail",
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo
    },
    data() {
      return {
        iid : null,
        topImages: [],
        goods: {},
        shop: {}
      }
    },
    created() {
      //保存传入的iid
      this.iid = this.$route.params.iid
      //根据iid查到数据
      getDeatil(this.iid).then(res => {
        const data = res.result;
        console.log(res)
        this.topImages = data.itemInfo.topImages;
        console.log(this.topImages)

         // 2.获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)

        // 3.创建店铺信息的对象
        this.shop = new Shop(data.shopInfo)


      })

     
    },
    methods: {
      
		 
    }
  }
</script>

<style scoped>
 
</style>