<template>
  <div class="goods-item">
      <img :src="goodsItem.show.img" alt="">
      <div>
          <p>{{goodsItem.title}}</p>
          <span>{{goodsItem.price}}</span>
          <span>{{goodsItem.cfav}}</span>
      </div>
  </div>
</template>

<script>
export default {
    name:"GoodsListItem",
    props:{
        goodsItem:{
            type:Object,
            default(){
                return []
            }
        }
    }
}
</script>

<style>
.goods-item{}
.goods-item img{width: 100%;height: 15rem;}
.goods-item div p{white-space: nowrap;display: block;overflow: hidden;text-overflow: ellipsis;}
</style>