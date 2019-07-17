<template>
<div class="container">
  <div class="header">
    <div class="left">
      <span class="iconfont icon-left" @click="back"></span>
    </div>
    <div class="middle">
      <ul>
        <li>商品</li>
        <li>详情</li>
        <li>评论</li>
        <li>推荐</li>
      </ul>
    </div>
    <div class="right">
      <span class="iconfont icon-menu"></span>
    </div>
  </div>
    <div class="content">
      <div class="banner">
        <van-swipe :autoplay="3000" indicator-color="white" class="bannerbox">
         <van-swipe-item v-for="(item, index) of bannerlist" :key = "index">
           <img :src="item">
         </van-swipe-item>
       </van-swipe>
      </div>
      <div class="info">
        <img src="http://img1.imgtn.bdimg.com/it/u=1859477950,3021007741&fm=26&gp=0.jpg" alt="">{{ info }}
      </div>
    </div>
    <van-goods-action>
    <van-sku
      v-model="show"
      stepper-title="我要买"
      :sku="sku"
      :goods="goods"
      show-add-cart-btn
      reset-stepper-on-hide
      :initial-sku="initialSku"
    >
    </van-sku>
  <van-goods-action-icon
    icon="chat-o"
    text="客服"
    @click="chart"
  />
  <van-goods-action-icon
    info="5"
    icon="cart-o"
    text="去购物车"
    @click="goCart"
  />
  <van-goods-action-icon
    icon="shop-o"
    text="店铺"
    @click="goShop"
  />
  <van-goods-action-button
    type="warning"
    text="加入购物车"
    @click="addCart"
  />
  <van-goods-action-button
    type="danger"
    text="立即购买"
    @click="buy"
  />
</van-goods-action>
  </div>
</template>
<script>
import Vue from 'vue'
import { Swipe, SwipeItem, GoodsAction, GoodsActionIcon, GoodsActionButton, Sku, NavBar } from 'vant'
Vue.use(GoodsAction).use(GoodsActionIcon).use(GoodsActionButton)
Vue.use(Sku)
Vue.use(NavBar)
Vue.use(Swipe).use(SwipeItem)
export default {
  data () {
    return {
      url: '',
      title: '',
      rating: '',
      list: {},
      a: {},
      info: '',
      bannerlist: [],
      show: false,
      initialSku: {
        // 键：skuKeyStr（sku 组合列表中当前类目对应的 key 值）
        // 值：skuValueId（规格值 id）
        s1: '30349',
        s2: '1193',
        // 初始选中数量
        selectedNum: 3
      },
      goods: {
        // 商品标题
        title: '测试商品',
        // 默认商品 sku 缩略图
        picture: 'https://img.yzcdn.cn/1.jpg'
      },
      sku: {
        // 所有sku规格类目与其值的从属关系，比如商品有颜色和尺码两大类规格，颜色下面又有红色和蓝色两个规格值。
        // 可以理解为一个商品可以有多个规格类目，一个规格类目下可以有多个规格值。
        tree: [
          {
            k: '颜色', // skuKeyName：规格类目名称
            v: [
              {
                id: '30349', // skuValueId：规格值 id
                name: '红色', // skuValueName：规格值名称
                imgUrl: 'https://img.yzcdn.cn/1.jpg' // 规格类目图片，只有第一个规格类目可以定义图片
              },
              {
                id: '1215',
                name: '蓝色',
                imgUrl: 'https://img.yzcdn.cn/2.jpg'
              }
            ],
            k_s: 's1' // skuKeyStr：sku 组合列表（下方 list）中当前类目对应的 key 值，value 值会是从属于当前类目的一个规格值 id
          }
        ],
        // 所有 sku 的组合列表，比如红色、M 码为一个 sku 组合，红色、S 码为另一个组合
        list: [
          {
            id: 2259, // skuId，下单时后端需要
            price: 100, // 价格（单位分）
            s1: '1215', // 规格类目 k_s 为 s1 的对应规格值 id
            s2: '1193', // 规格类目 k_s 为 s2 的对应规格值 id
            s3: '0', // 最多包含3个规格值，为0表示不存在该规格
            stock_num: 110 // 当前 sku 组合对应的库存
          }
        ],
        price: '1.00', // 默认价格（单位元）
        stock_num: 227, // 商品总库存
        collection_id: 2261, // 无规格商品 skuId 取 collection_id，否则取所选 sku 组合对应的 id
        none_sku: false, // 是否无规格商品
        messages: [
          {
            // 商品留言
            datetime: '0', // 留言类型为 time 时，是否含日期。'1' 表示包含
            multiple: '0', // 留言类型为 text 时，是否多行文本。'1' 表示多行
            name: '留言', // 留言名称
            type: 'text', // 留言类型，可选: id_no（身份证）, text, tel, date, time, email
            required: '1' // 是否必填 '1' 表示必填
          }
        ],
        hide_stock: false // 是否隐藏剩余库存
      }
    }
  },
  created () {
    this.url = window.location.href
    // console.log(this.url.substring(29))
    fetch('http://10.11.56.121:3000/124').then(res => res.json()).then(data => {
      this.a = data[0].data
      // console.log(data[0].data)
      for (this.i in data[0].data) {
        // console.log(data[0].data[this.i])
        for (let j = 0; j < data[0].data[this.i].length; j++) {
          // console.log(Math.round(this.url.substring(29)), data[0].data[this.i][j].goods_id)
          if (Math.round(this.url.substring(29)) === data[0].data[this.i][j].goods_id) {
            this.list = data[0].data[this.i][j]
            this.goods.title = this.list.short_name
            this.goods.picture = this.list.src
            this.info = this.list.goods_name
            this.list.price = this.list.group_price
            console.log(typeof this.info)
            this.bannerlist = this.list.Commoditydetails_img.split(' ')
            console.log(this.goods.picture)
          }
        }
      }
    })
  },
  methods: {
    chart () {
      console.log('客服')
    },
    goCart () {
      console.log('去购物车')
    },
    goShop () {
      console.log('店铺')
    },
    addCart () {
      console.log('加入购物车')
      this.show = true
    },
    buy () {
      console.log('立即购买')
    },
    back () {
      this.$router.back()
    },
    share () {
      console.log('分享')
    }
  }
}
</script>
<style lang="scss">
@import '@/lib/reset.scss';
.banner{
  height:3.8rem;
  .bannerbox{
    height:100%;
  }
  img{
    width:100%;
  }
  }
  .info{
    img{
      width:6%;
    }
  }
  .middle{
    li{
      float: left;
      margin-left:10px;
      font-size:0.2rem;
    }
  }
</style>
