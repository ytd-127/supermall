<template>
  <div class="wrapper-a">
      <div class="cont">
          <slot></slot>
      </div>
  </div>
</template>

<script>
export default {
    name:'Scroll',
    data(){
        return {
            scroll:null
        }
    },
    mounted(){
        this.scroll = new BScroll(document.querySelector(''))
    }
}
</script>

<style>

</style>