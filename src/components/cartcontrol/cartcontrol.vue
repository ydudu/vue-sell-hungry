<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0"  @click.stop.prevent="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script>
    import Vue from 'vue';

    export default {
        name: "cartcontrol",
        props: {
          food: {
            type: Object
          }
        },
        methods: {
          addCart(event) {
            if(!event._constructed) { //去掉自带的click事件点击，即pc端直接返回
              return;
            }
            if(!this.food.count){
               Vue.set(this.food, 'count', 1);//给this.food增加一个count属性，并初始化为1
            }else{
              this.food.count++;
            }
            //$emit触发父组件的自定义事件
            this.$emit('add', event.target); //$emit, $on, $off 分别来分发、监听、取消监听事件：
          },
          decreaseCart() {
            if(!event._constructed) {
              return;
            }
            if(this.food.count) {
              this.food.count--;
            }
          }
        }
    }
</script>

<style lang="stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease
      display: inline-block
      opacity: 1
      padding: 6px
      transform: translate3d(0, 0, 0)
      .inner
        display: inline-block
        font-size: 24px
        line-height: 24px
        color: rgb(0, 160, 220)
        transition: all 0.4s linear
        transfrom: rotate(0)
      &.move-enter-active, &.move-leave-active
        transition: all 0.4s linear
      &.move-enter, &.move-leave-active
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(360deg)
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
      padding: 6px
      font-size: 24px
      line-height: 24px
      color: rgb(0, 160, 220)
</style>
