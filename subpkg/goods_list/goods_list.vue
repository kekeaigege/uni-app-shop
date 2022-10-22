<template name="my-goods">
  <view>
    <view>
      <view class="goods-list">
        <block v-for="(goods, i) in goodsList" :key="i">
          <my-goods :goods="goods"></my-goods>
        </block>
      </view>
    </view>
  </view>
</template>

<script>

  export default {
    data() {
      return {
        isLoading: false,
        // 请求参数对象
        queryObj: {
          // 查询关键词
          query: '',
          // 商品分类Id
          cid: '',
          // 页码值
          pagenum: 1,
          // 每页显示多少条数据
          pagesize: 10
        },
        // 商品列表的数据
        goodsList: [],
        // 总数量，用来实现分页
        total: 0
      }
    },

    onLoad(options) {
      this.queryObj.query = options.query || '';
      //三级页面跳转
      this.queryObj.cid = options.cid || '';
      this.getGoodsList()
    },
    methods: {
      // 获取商品列表数据的方法
      async getGoodsList(callback) {
        this.isLoading = true;
        // 发起请求
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
        if (res.meta.status !== 200) return uni.$showMsg()
        // 为数据赋值
        this.goodsList = [...this.goodsList, res.message.goods]
        this.total = res.message.total

        console.log("商品列表信息")
        console.log(this.goodsList)
        this.isLoading = false;
        callback && callback();
      }
    },
    onReachBottom() {
      if (this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完毕！')
      if (isLoading) return;
      this.queryObj.pagenum += 1;
      this.getGoodsList()
    },
    onPullDownRefresh() {
      // 1. 重置关键数据
      this.queryObj.pagenum = 1
      this.total = 0
      this.isloading = false
      this.goodsList = []
      // 2. 重新发起请求
      this.getGoodsList(() => uni.stopPullDownRefresh())
    }

  }
</script>

<style lang="scss">
 
</style>
