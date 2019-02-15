<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count > 0" @click="decreaseCart">
        <span class="inner icon-minus"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="cart-add icon-plus" @click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  export default {
    name: 'cartcontrol',
    props: {
      food: {
        type: Object
      }
    },
    created () {
      // console.log(this.food)
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        // console.log('clicked')
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease
      display: inline-block
      padding: 6px
      font-size: 0
      transition: all .3s linear;
      .inner
        position: relative
        top: -1px
        display inline-block
        font-size: 22px
        line-height: 22px
        color: rgb(0, 160, 220)
        transition: all .3s linear;
      &.move-enter-active, &.move-leave-active
        transform: translate3d(0, 0, 0)
        opacity: 1
        .inner
          transform: rotate(0)
          opacity: 1
      &.move-enter, &.move-leave-to /* .fade-leave-active below version 2.1.8 */
        transform: translate3d(22px, 0, 0)
        opacity: 0
        .inner
          transform: rotate(180deg)
          opacity: 1
    .cart-count
      display: inline-block
      min-width: 12px
      vertical-align: top
      padding-top: 6px
      line-height: 24px
      font-size: 12px
      text-align: center
    .cart-add
      display: inline-block
      padding: 6px
      font-size: 24px
      line-height: 24px
      color: rgb(0, 160, 220)
</style>
