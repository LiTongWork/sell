<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" :key="index" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index)">
          <span class="text border-1px">
            <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="food-list food-list-hook" :key="index">
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
                  <span class="now">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <CartControl :food="food"></CartControl>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <ShopCart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></ShopCart>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import ShopCart from '../shopcart/shopcart'
  import CartControl from '../cartcontrol/cartcontrol'
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
        goods: [],
        listHeight: [],
        scrollY: 0
      }
    },
    components: {
      ShopCart,
      CartControl
    },
    created () {
      this.$http.get('/api/goods').then((response) => {
        if (response.body.erron === errok) {
          this.goods = response.body.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          })
        }
      });
      this.classMap = ['decrease', 'discount', 'special', 'guarantee', 'invoice']
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            // console.log(i);
            return i
          }
        }
        return 0
      },
      selectFoods () {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          })
        });
        return foods;
      }
    },
    methods: {
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _calculateHeight () {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu (index) {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodScroll.scrollToElement(el, 260);
        console.log(index)
      }
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
      /*overflow-x hidden*/
      /*overflow-y: auto*/
      .menu-item
        display: table
        height: 54px
        width: 56px
        line-height: 14px
        font-size: 14px
        margin-left: 12px
        &.current
          background-color: #fff
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
    /*overflow-y: auto*/
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
          line-height: 12px
          color: rgb(147,153,159)
          margin-top: 8px
        .extra
          .count
            margin-right: 12px
        .price
          font-size: 14px
          margin-top: 12px
          color: rgb(240,20,20)
          .now
            margin-right: 8px
            line-height: 14px
            font-weight: 700
          .old
            font-size: 10px
            line-height: 14px
            font-weight: 700
            color: rgb(147,153,159)
            text-decoration: line-through
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 10px
</style>
