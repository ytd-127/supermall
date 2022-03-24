<template>
  <div class="wrapper-a" ref="wrapper">
      <div class="cont">
          <slot></slot>
      </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
    name:'Scroll',
    data(){
        return {
            scroll:null
        }
    },
    mounted(){
        this.scroll = new BScroll(this.$refs.wrapper,{
            probeType:3,
            pullUpLoad:true,
            click:true
        })
        this.scroll.on('scroll',(position)=>{
            console.log(position);
        })

        this.scroll.on('pullingUp',()=>{
            console.log('上拉加载更多');

            setTimeout(() => {
                this.scroll.finishPullUp()
            },2000)
        })
    },
    methods:{
        scrollTo(x,y,time=300){
            this.scroll.scrollTo(x,y,time)
        }
    }
}
</script>

<style>

</style>