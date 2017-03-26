<template>
  <div id="app">
    <app-header v-bind:seller="seller"></app-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
  import {urlParse} from 'common/js/util'
  import AppHeader from 'components/header/header'

  const ERR_OK = 0
  export default {
    name: 'app',
    data () {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse()
            return queryParam.id
          })()
        }
      }
    },
    created () {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.seller = Object.assign({}, this.seller, response.data)
        }
      })
    },
    components: {
      AppHeader
    }
  }

</script>

<style lang="stylus" rel="stylesheet/stylus">
  html, body
    height: 100%
    #app
      position: relative
      height: 100%
      .tab
        width: 100%
        height: 40px
        line-height: 40px
        display: flex
        .tab-item
          flex: 1
          text-align: center
          a
            display: block
            color: rgb(77, 85, 93)
            &.router-link-active
              color: rgb(240, 20, 20)
</style>
