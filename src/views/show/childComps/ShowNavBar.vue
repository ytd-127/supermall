<template>
  <nav-bar>
      <div slot="left" @click="backClick">
          <img src="~assets/img/common/back.svg" alt="">
      </div>
      <div slot="center" class="center">
          <div v-for="(item,index) in title" :key="index" class="center-item" :class="{active : index === currentIndex}" @click="tabClick(index)">{{item}}</div>
      </div>
  </nav-bar>
</template>

<script>

import NavBar from 'components/common/navbar/NavBar'
export default {
    name:"ShowNavBar",
    components:{
        NavBar
    },
    data(){
        return {
            title:["商品","参数","评论","推荐"],
            currentIndex : 0
        }
    },
    methods:{
        tabClick(index){
            this.currentIndex = index;
        },
        backClick(){
            this.$router.back()
        }
    }
}
</script>

<style scoped>

.center{display: flex;}
.center-item{flex:1;}
.active{color:brown;}
</style>