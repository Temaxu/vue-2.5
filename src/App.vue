<template>
  <div id="app" >
    <v-header :seller="seller" @touchmove.prevent></v-header>
      <!-- 使用tab 组件 -->
      <div class="tab-wrapper">
         <tab :tabs="tabs"> </tab><!-- 用事件绑定多个组件 -->
      </div>
      <!--   使用tab 组件
    <tab></tab> -->
  </div>
</template>

<script>
  import Goods from 'components/goods/goods'
  import Ratings from 'components/ratings/ratings'
  import Seller from 'components/seller/seller'
  import VHeader from 'components/v-header/v-header.vue'
  // import qs from 'query-string'
  import { getSeller } from 'api'
  import Tab from './components/tab/tab'

  export default {
    name: 'app',
    data() {
      return {
        seller: {}
      }
    },
    computed: {
      tabs() {
        return [ // 返回一个数组
          {
           label: '商品',
            component: Goods, // 引用组件
            data: {
              seller: this.seller // data里的是一个对象
            }
          },
          {
            label: '评价',
            component: Ratings,
            data: {// data里的是一个对象
              seller: this.seller
            }
          },
          {
            label: '商家',
            component: Seller,
            data: {
              seller: this.seller
            }
          }
        ]
      }
    },
    // computed: {
    //   tabs() {
    //     return [
    //       {
    //         label: '商品',
    //         component: Goods,
    //         data: {
    //           seller: this.seller
    //         }
    //       },
    //       {
    //         label: '评论',
    //         component: Ratings,
    //         data: {
    //           seller: this.seller
    //         }
    //       },
    //       {
    //         label: '商家',
    //         component: Seller,
    //         data: {
    //           seller: this.seller
    //         }
    //       }
    //     ]
    //   }
    // },
    created() {
      this._getSeller()
    },
    methods: {
      _getSeller() {
        getSeller({
          id: this.seller.id
        }).then((seller) => {
          this.seller = Object.assign({}, this.seller, seller)
        })
      }
    },
    components: {
      VHeader,
      Tab
      // Goods,
      // Ratings,
      // Seller,
    }
  }
</script>

<style  lang="stylus" scoped>
  #app
    .tab-wrapper
      position: fixed
      top: 136px
      left: 0
      right: 0
      bottom: 0
</style>
