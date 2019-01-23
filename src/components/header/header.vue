<template>
  <div class="v-header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-right"></i>
      </div>
    </div>
  </div>
</template>

<script>
    export default {
      name: 'v-header',
      props: {
        seller: {
          type: Object
        }
      },
      created () {
        this.classMap = ['decrease', 'discount', 'special', 'guarantee', 'invoice']
      }
    }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
.v-header
  color: #fff
  background-color: #ccc
  .content-wrapper
    position: relative
    padding: 24px 12px 18px 24px
    font-size: 0
    .avatar
      display: inline-block
      vertical-align: top
      img
        border-radius: 2px
    .content
      display inline-block
      margin-left: 16px
      .title
        /*font-size:*/
        margin: 2px 0 8px
        .brand
          display: inline-block
          vertical-align: top
          width: 30px
          height: 18px
          bg-image:(brand)
          background-size: 30px 18px
          background-repeat: no-repeat
        .name
          margin-left: 6px
          font-size: 16px
          font-weight: bold
      .description
        margin-bottom: 10px
        font-size: 12px
      .support
        .icon
          display: inline-block
          vertical-align top
          width: 12px
          height: 12px
          margin-right: 4px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image(decrease_1)
          &.discount
            bg-image(decrease_1)
          &.guarantee
            bg-image(guarantee_1)
          &.invoice
            bg-image(invoice_1)
          &.special
            bg-image(special_1)
        .text
          font-size: 10px
    .support-count
      position: absolute
      right: 12px
      bottom: 18px
      font-size: 10px
      height: 24px
      line-height 24px
      padding: 0 8px
      border-radius: 14px
      background-color: rgba(0,0,0,.2)
</style>
