<template>
  <div class="goods">
      <goods-list-item v-for="(item,index) in goods" :key="index" :goodsItem="item"/>
  </div>
</template>

<script>
    import GoodsListItem from './GoodsListItem.vue'

export default {
    name:"GoodsList",
    components:{
        GoodsListItem
    },
    props:{
        goods:{
            goods:{
                type:Array,
                default(){
                    return []
                }
            }
        }
    }
}
</script>

<style>
.goods{display: flex;width: 100vw;overflow: hidden;flex-wrap: wrap;justify-content: space-between;padding: 0 2%;box-sizing: border-box;}
.goods-item{width: 49%;}
</style>