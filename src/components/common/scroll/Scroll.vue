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
    props:{
        probeType:{
            type:Number,
            default:0
        },
        pullUpLoad:{
            type:Boolean,
            default:false
        }
    },
    data(){
        return {
            scroll:null
        }
    },
    mounted(){
        this.scroll = new BScroll(this.$refs.wrapper,{
            probeType:this.probeType,
            pullUpLoad:this.pullUpLoad,
            click:true
        })
        this.scroll.on('scroll',(position)=>{
            // console.log(position);
            this.$emit('scroll',position)
        })

        this.scroll.on('pullingUp',()=>{
            this.$emit('pullingUp')
            setTimeout(() => {
                this.scroll.finishPullUp()
            },2000)
        })
    },
    methods:{
        scrollTo(x,y,time=300){
            this.scroll.scrollTo(x,y,time)
        },
        refresh(){
            this.scroll && this.scroll.refresh();
        }
    }
}
</script>

<style>

</style>