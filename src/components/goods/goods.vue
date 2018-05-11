<template>
  <div class='goods'>
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':index===currentIndex}" ref="menuList"
            @click="selectMenu(index)">
          <span class="name"><span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span> {{item.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="item in goods" class="good-item food-list-hook" ref="foodList" >
          <h2 class="title">{{item.name}}</h2>
          <ul>
            <li v-for="food in item.foods" class="food-item" @click="foodDetail(food)">
              <div class="icon">
                <img :src="food.icon" width="58" height="58" alt="">
              </div>
              <div class="content">
                <div class="name">{{food.name}}</div>
                <div class="desc" v-show="food.description">{{food.description}}</div>
                <div class="sell">
                  <span class="count">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">¥{{food.price}}</span><span class="old"
                                                                v-show="food.oldPrice">¥{{food.oldPrice}}</span>
                </div>
              </div>
              <div class="cartcontrol-wrapper">
                <cart-control :food="food" @add="addFood"></cart-control>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <div class="shopcart-wrapper">
      <shopcart :selectFoods="selectFoods" :min-price="seller.minPrice"
                :delivery-price="seller.deliveryPrice" ref="shopcart"></shopcart>
    </div>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script>
  import axios from 'axios'
  import BScroll from 'better-scroll'
  import CartControl from "../cart-control/cart-control";
  import Shopcart from "../shopcart/shopcart";
  import Food from "../food/food";
  const debug = process.env.NODE_ENV !== 'production';
  export default {
    components: {
      Food,
      Shopcart,
      CartControl
    },
    data() {
      return {
        goods: {},
        listHeight: [],
        scrollY: 0,
        selectedFood:{}
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];

      const url = debug ? '/api/goods' : 'http://ustbhuangyi.com/sell/api/goods';
      axios.get(url).then((res) => {
        let result = res.data;
        if (result.code === 0) {
          this.goods = result.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          })
        }
      })
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY <= height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        for(let i=0;i<this.goods.length;i++){
          let good = this.goods[i];
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        }
        return foods;
      }
    },

    methods: {
      foodDetail(food){
        this.selectedFood = food;
        this.$refs.food.show();
      },
      /*两个区块分别能够滚动*/
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        })
        this.foodScroll = new BScroll(this.$refs.foodWrapper, {
          click: true,
          probeType: 3
        })
        this.foodScroll.on('scroll', (pos) => {
          if (pos.y <= 0) {
            this.scrollY = Math.abs(Math.round(pos.y))
          }
        })
      },
      /*计算每个food区块的高度*/
      _calculateHeight() {
        let height = 0;
        let foodList = this.$refs.foodList;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      /*点击滚动*/
      selectMenu(index) {
        let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodScroll.scrollToElement(el, 300);
      },
      addFood(target){
        this._drop(target)
      },
      _drop(target){
        this.$nextTick(()=>{
          this.$refs.shopcart.drop(target);
        })
      }
    }
  }
</script>

<style lang='scss' scoped>
  @import "./../../assets/scss/mixin";

  .goods {
    position: absolute;
    top: 174px;
    left: 0;
    right: 0;
    display: flex;
    bottom: 48px;
    overflow: hidden;
    .menu-wrapper {
      flex: 0 0 80px;
      width: 80px;
      background: #f3f5f7;
      ul {
        list-style: none;
        padding-left: 0;
        .menu-item {
          width: 56px;
          padding: 0 12px;
          line-height: 14px;
          height: 54px;
          display: table;
          font-weight: 500;
          &.current {
            position: relative;
            z-index: 10;
            margin-top: -1px;
            background: #fff;
            font-weight: 700;
          }
          .icon {
            vertical-align: top;
            display: inline-block;
            width: 12px;
            height: 12px;
            background-size: 12px 12px;
            &.discount {
              @include bg-image('discount_3')
            }
            &.decrease {
              @include bg-image('decrease_3')
            }
            &.guarantee {
              @include bg-image('guarantee_3')
            }
            &.invoice {
              @include bg-image('invoice_3')
            }
            &.special {
              @include bg-image('special_3')
            }
          }
          .name {
            display: table-cell;
            width: 56px;
            vertical-align: middle;
            @include border-1px(rgba(7, 17, 27, 0.1));
            font-size: 12px;
          }
        }
      }
    }
    .foods-wrapper {
      flex: 1;
      > ul {
        margin-top: -10px;
        .good-item {
          .title {
            position: relative;
            width: 100%;
            padding-left: 14px;
            box-sizing: border-box;
            height: 26px;
            font-size: 12px;
            color: rgb(147, 153, 159);
            line-height: 26px;
            background: #f3f5f7;
            &::before {
              position: absolute;
              content: '';
              left: 0;
              top: 0;
              width: 2px;
              height: 26px;
              background: #d9dde1;
            }
          }
          .food-item {
            position: relative;
            padding: 18px;
            border-bottom: 1px solid rgba(7, 17, 27, .1);
            font-size: 0;
            font-weight: normal;
            display: flex;
            &:last-child {
              border-bottom: none;
            }
            .icon {
              flex: 0 0 58px;
              width: 58px;
              height: 58px;
              margin-right: 10px;
            }
            .content {
              flex: 1;
              vertical-align: top;
              .name {
                font-size: 14px;
                color: rgb(7, 17, 27);
                line-height: 14px;
              }
              .desc {
                font-size: 10px;
                color: rgb(147, 153, 159);
                line-height: 10px;
                margin-top: 9px;
              }
              .sell {
                margin-top: 8px;
                font-size: 10px;
                color: rgb(147, 153, 159);
                line-height: 10px;
                .rating {
                  margin-left: 12px;
                }
              }
              .price {
                margin-top: 8px;
                font-size: 14px;
                color: rgb(220, 20, 20);
                font-weight: 700;
                .old {
                  margin-left: 8px;
                  font-size: 10px;
                  color: rgb(147, 153, 159);
                }
              }
            }
            .cartcontrol-wrapper {
              position: absolute;
              right: 20px;
              bottom: 0;
            }
          }
        }
      }
    }
    .shopcart-wrapper {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 44px;
    }
  }
</style>
