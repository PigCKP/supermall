<template>
<!--子组件绑定ref,普通的div也可以绑定ref-->
  <div class="wrapper" ref="wrapper">
      <div class="content">
          <slot></slot>
      </div>
  </div>
</template>

<script>
    import BScroll from 'better-scroll'
    export default {
        name: "Scroll",
        props: {
            probeType: {
                type: Number,
                default: 0
            },
            pullUpLoad: {
                type: Boolean,
                default: false
            }
        },
        data(){
            return{
                scroll :null,
            }
        },
        mounted(){
            // 1、创建better-scroll
            this.scroll = new BScroll(this.$refs.wrapper,{
                click :true,
                probeType: this.probeType,
                pullUpLoad: this.pullUpLoad
                // probeType:3
            })
            //2、监听滚动位置
            this.scroll.on('scroll',(position) => {
                //  console.log(position)
                //  this.scroll.scrollTo(0,0)
                 this.$emit('scroll',position)
            })
           
           //3、监听上啦加载更多
           this.scroll.on('pullingUp',() => {
            //    console.log('上啦记载')
               this.$emit('pullingUp')
           })

            // console.log(this.scroll)
            // console.log(this.scroll.scrollerHeight)
            // this.scroll.refresh()
        },
        methods: {
            scrollTo(x,y,time=3000){
                this.scroll && this.scroll.scrollTo && this.scroll.scrollTo(x,y,time)
            },
            finishPullUp(){
                this.scroll.finishPullUp()
            },
            refresh(){
                this.scroll && this.scroll.refresh()
            }
        }
    }
</script>

<style scoped>
  
</style>
