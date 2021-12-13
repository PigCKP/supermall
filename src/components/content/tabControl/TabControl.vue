<template>
  <div class="tab-control">
     <div v-for="(item,index) in titles" :key="index" 
     class="tab-control-item" :class="{active:index == currentIndex}" @click="tabClick(index)"
     >
         <span>{{item}}</span>
     </div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    props:{
        titles:{
            type: Array,
            default(){
                return [];
            }
        }
    },
    data(){
        return{
            currentIndex:0,
        }
    },
    methods:{
        tabClick(index){
            this.currentIndex = index;
        }
    }
 
  }
</script>

<style scoped>
    .tab-control{
        width:100%;
        height:50px;
        text-align:center;
        line-height:50px;
        display:flex;
        font-size:15px;
        background:#fff;
    }
    .tab-control-item{
        /* float:left; */
        flex:1;
     }
     .active{
          color: var(--color-tint);
     }
      .active span{
        border-bottom: 3px solid var(--color-tint)
      }
</style>
