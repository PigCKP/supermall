<template>
  <div class="goodlist">
      <h3>{{goods}}</h3>
  </div>
</template>

<script>
  export default {
    name: "GoodsList",
    props:{
        goods:{
            type:Array,
            default(){
                return []
            }
        }
    }
  }
</script>

<style scoped>
  
</style>
