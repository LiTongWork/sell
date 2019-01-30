<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(item,index) in goods" :key="index" class="menu-item">
          <span class="text border-1px">
            <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script>
  const errok = 0;
  export default {
    name: 'goods',
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: ''
      }
    },
    created () {
      this.$http.get('/api/goods').then((response) => {
        if (response.body.erron === errok) {
          this.goods = response.body.data;
        }
      });
      this.classMap = ['decrease', 'discount', 'special', 'guarantee', 'invoice']
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background-color: #f3f5f7
      overflow-x hidden
      overflow-y: auto
      .menu-item
        display: table
        height: 54px
        width: 56px
        line-height: 14px
        font-size: 14px
        margin-left: 12px
        .text
          display table-cell
          width: 56px
          font-size: 12px
          vertical-align: middle
          border-1px(rgba(7,17,27,.1))
          .icon
            display: inline-block
            vertical-align top
            width: 12px
            height: 12px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image('decrease_3')
            &.discount
              bg-image('discount_3')
            &.guarantee
              bg-image('guarantee_3')
            &.invoice
              bg-image('invoice_3')
            &.special
              bg-image('special_3')
  .foods-wrapper
      flex: 1
</style>
