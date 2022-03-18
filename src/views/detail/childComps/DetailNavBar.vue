<template>
  <div id="detail">
    <nav-bar>
      <div slot="center" class="title">
        <div slot="left" class="back_svg" @click="backClick">
          <img src="~assets/img/common/back.svg" alt="">
        </div>
        <div  v-for="(item,index) in titles" :key="index" :class="{active : index == currendex}" class="class-item" @click="itemClick(index)">
           {{item}}
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar.vue'
  export default {
    name: "DetailNabBar",
    components: {
      NavBar
    },
    data() {
      return {
       titles: ['商品','参数','评论','推荐'],
       currendex:0
      }
    },
    created() {
      this.iid = this.$route.params.iid
    },
    methods: {
      itemClick(index){
        this.currendex = index;
      },
      backClick(){
        this.$router.back()
      }
		 
    }
  }
</script>

<style scoped>
 
.title{
  display: flex;
  font-size:13px;
}
.class-item{
  flex: 1;
}
.active{
  color: red;
}
.back_svg img{
  margin-top: 12px;
  width:20px;

}

</style>