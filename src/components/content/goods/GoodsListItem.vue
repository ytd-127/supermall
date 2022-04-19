<template>
  <div class="goods-item" @click="linkClick">
      <img :src="goodsItem.show.img" alt="" @load="imagesLoad">
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
    },
    created(){
        // this.$ebus.$emit('imagesLoad')
    },
    methods:{
        imagesLoad(){
            this.$bus.$emit('imagesLoad');
        },
        linkClick(){
            this.$router.push("/show/"+this.goodsItem.iid)
        }
    }
}
</script>

<style>
.goods-item{}
.goods-item img{width: 100%;height: 15rem;}
.goods-item div p{white-space: nowrap;display: block;overflow: hidden;text-overflow: ellipsis;}
</style>