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
    <div class="foods-wrapper">
      <ul>
        <li v-for="(item,index) in goods" class="food-list" :key="index">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="(food,index) in item.foods" class="food-item border-1px" :key="index">
              <div class="icon">
                <img :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="new">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
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
    overflow-y: auto
    .title
      padding-left: 12px
      height: 26px
      line-height: 26px
      font-size: 12px
      color: rgb(147,153,159)
      background-color: #f3f5f7;
      border-left: 2px solid #d9dde1
    .food-item
      display: flex
      margin: 18px 18px 0
      padding-bottom: 18px
      border-1px(rgba(7,17,27,.1))
      &:last-child
        border-none()
      .icon
        flex: 0 0 57px
        width: 57px
        height: 57px
        margin-right: 10px
        img
          width: 100%
          height: 100%
      .content
        flex: 1
        .name
          font-size: 14px
          line-height: 14px
          color: rgb(7,17,27)
          margin-top: 2px
        .desc,.extra
          font-size: 10px
          line-height: 10px
          color: rgb(147,153,159)
          margin-top: 8px
        .extra
          .count
            margin-right: 12px
        .price
          font-size: 14px
          line-height: 14px
          margin-top: 8px
          color: red
          .new
            margin-right: 8px
          .old
            font-size: 10px
            color: rgb(147,153,159)
</style>
