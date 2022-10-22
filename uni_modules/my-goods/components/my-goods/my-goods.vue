<template>
  <view>
    <view v-for="(item,i) in goods" :key="i" @click="gotoDetail(item)">
      <view class="goods-item">
        <view class="goods-item-left">
          <image :src="item.goods_small_logo || defaultPic" class="goods-pic"></image>
        </view>
        <!-- 商品右侧信息区域 -->
        <view class="goods-item-right">
          <!-- 商品标题 -->
          <view class="goods-name">{{item.goods_name||''}}</view>
          <view class="goods-info-box">
            <!-- 商品价格 -->
            <view class="goods-price">￥{{item.goods_price | toFixed}}</view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        defaultPic: 'https://img3.doubanio.com/f/movie/8dd0c794499fe925ae2ae89ee30cd225750457b4/pics/movie/celebrity-default-medium.png',
      }
    },
    props: {
      goods: {
        type: Array,
        default: []
      },
    },
    filters: {
      toFixed(num) {
        return Number(num).toFixed(2)
      }
    },
    methods:{
      gotoDetail(item){
        uni.navigateTo({
           url:'/subpkg/good_detail/good_detail?goods_id=' + item.goods_id
        })
      }
    },

  }
</script>

<style lang="scss">
  .goods-item {
    display: flex;
    padding: 10px 5px;
    border-bottom: 1px solid #f0f0f0;
    .goods-item-left {
      margin-right: 5px;

      .goods-pic {
        width: 100px;
        height: 100px;
        display: block;
      }
    }

    .goods-item-right {
      display: flex;
      flex-direction: column;
      justify-content: space-between;

      .goods-name {
        font-size: 13px;
      }

      .goods-price {
        font-size: 16px;
        color: #c00000;
      }
    }
  }
</style>
