<template>
  <div class="show">
      <show-nav-bar></show-nav-bar>
      <div>{{iid}}</div>
  </div>
</template>


<script>

import ShowNavBar from "./childComps/ShowNavBar"

export default {
    name:"Show",
    components:{
        ShowNavBar
    },
    data(){
        return{
            iid : null
        }
    },
    created(){
        this.iid = this.$route.params.iid
    }
}
</script>

<style>

</style>