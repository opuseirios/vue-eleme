<template>
  <transition name="slide">
    <div class='food' v-show="showFlag" ref="food">
      <div class="food-content">
        <div class="img">
          <img :src="food.image" alt="">
          <div class="back" @click="back">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h2 class="name">{{food.name}}</h2>
          <div class="sell">
            <span class="count">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">¥{{food.price}}</span><span class="old" v-show="food.oldPrice">{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <div class="addCart" v-if="!food.count" @click="addCart(food)">加入购物车</div>
            <cart-control :food="food" v-else></cart-control>
          </div>
        </div>
        <split></split>
        <div class="info" v-show="food.info">
          <h2 class="name">商品介绍</h2>
          <div class="text">{{food.info}}</div>
        </div>
        <split v-show="food.info"></split>
        <div class="rating">
          <h2 class="title">商品评价</h2>
          <rating-select @toggle="toggleContent" @select="selectRating" :only-content="onlyContent" :ratings="food.ratings" :desc="desc"></rating-select>
          <div class="rating-wrapper">
            <ul v-show="food.ratings&&food.ratings.length">
               <li v-for="rating in food.ratings" class="rating-item border-1px" v-show="needShow(rating.rateType,rating.text)">
                 <div class="user">
                   <span class="name">{{rating.username}}</span>
                   <img :src="rating.avatar" alt="" width="12" height="12" class="avatar">
                 </div>
                 <div class="time">{{rating.rateTime}}</div>
                 <p class="text">
                   <span :class="rating.rateType===0?'icon-thumb_up':'icon-thumb-down'"></span>{{rating.text}}
                 </p>
               </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
              暂无评价
            </div>
          </div>
        </div>
      </div>

    </div>
  </transition>
</template>

<script>
  import CartControl from "../cart-control/cart-control";
  import Split from "../split/split";
  import BScroll from 'better-scroll'
  import Vue from 'vue'
  import RatingSelect from "../rating-select/rating-select";

  const ALL = 2;
  export default {
    components: {
      RatingSelect,
      Split,
      CartControl
    },
    name: 'food',
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false,
        desc: {
          'all': '全部',
          'positive': '推荐',
          'negative': '吐槽·'
        },
        onlyContent: true,
        selectType: ALL,
      }
    },
    methods: {
      show() {
        this.showFlag = true;
        this.onlyContent = true;
        this.selectType = ALL;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      back() {
        this.showFlag = false;
      },
      addCart(food) {
        if (!food.count) {
          Vue.set(food, 'count', 1)
        }
      },
      toggleContent(){
        this.onlyContent = !this.onlyContent;

        this.$nextTick(()=>{
          this.scroll.refresh();
        })
      },
      needShow(type,text){
        if(this.onlyContent && !text){
          return false
        }
        if(this.selectType === ALL){
          return true
        }else {
          return type === this.selectType
        }
      },
      selectRating(type) {
        this.selectType = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
    }

  }
</script>

<style lang='scss' scoped>
  @import "./../../assets/scss/mixin";
  .food {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 80;
    background: #fff;
    &.slide-enter-active, &.slide-leave-active {
      transition: all .5s;
    }
    &.slide-enter, &.slide-leave-to {
      left: 100%;
    }
    .img {
      width: 100%;
      height: 0;
      padding-top: 100%;
      position: relative;
      img {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
      }
      .back {
        position: absolute;
        top: 10px;
        left: 0;
        .icon-arrow_lift {
          display: block;
          padding: 10px;
          font-size: 20px;
          color: #fff;
        }
      }
    }
    .content {
      position: relative;
      padding: 18px;
      .name {
        font-size: 14px;
        font-weight: 700;
        color: rgb(7, 17, 27);
        line-height: 14px;
      }
      .sell {
        margin-top: 8px;
        font-size: 0;
        line-height: 10px;
        color: rgb(147, 153, 159);
        font-weight: 500;
        .count {
          display: inline-block;
          font-size: 10px;
          margin-right: 12px;
        }
        .rating {
          padding: 0;
          display: inline-block;
          font-size: 10px;
        }
      }
      .price {
        margin-top: 18px;
        font-size: 0;
        .now {
          display: inline-block;
          font-size: 14px;
          font-weight: 700;
          color: rgb(240, 20, 20);
          line-height: 24px;
          margin-right: 12px;
        }
        .old {
          font-size: 10px;
          font-weight: 700;
          color: rgb(147, 153, 159);
          line-height: 24px;
        }
      }
      .cartcontrol-wrapper {
        position: absolute;
        right: 18px;
        bottom: 18px;
        .addCart {
          margin: 5px 0;
          font-size: 10px;
          color: #fff;
          line-height: 12px;
          padding: 6px 12px;
          border-radius: 12px;
          background-color: rgb(0, 160, 220);
        }
      }
    }
    .info {
      padding: 18px;
      .name {
        font-size: 14px;
        font-weight: 700;
        color: rgb(7, 17, 27);
        line-height: 14px;
      }
      .text {
        padding: 0 8px;
        margin-top: 6px;
        font-size: 12px;
        color: rgb(77, 85, 93);
        line-height: 24px;
      }
    }
    .rating {
      padding: 18px;
      .title {
        font-size: 14px;
        font-weight: 700;
        color: rgb(7, 17, 27);
        line-height: 14px;
      }
      .rating-wrapper{
        padding: 0 18px;
        .rating-item{
          position: relative;
          @include border-1px(rgba(7,17,27,.1));
          .time{
            float: left;
            font-size: 10px;
            color: rgb(147,153,159);
            line-height: 12px;
          }
          .user{
            float: right;
            .name{
              vertical-align: top;
              font-size: 10px;
              color: rgb(147,153,159);
              line-height: 12px;
              margin-right: 6px;
            }
            .avatar{
              vertical-align: top;
              border-radius: 50%;
            }
          }
          .text{
            padding: 16px 0;
            font-size: 12px;
            line-height: 16px;
            color: rgb(7,17,27);
            .icon-thumb_up{
              font-size: 12px;
              color: rgb(0,160,220);
              line-height: 24px;
              margin-right: 4px;
            }
            .icon-thumb_down{
              font-size: 12px;
              color: rgb(147,153,159);
              line-height: 24px;
              margin-right: 4px;
            }
          }
        }
        .no-rating{
          padding: 16px 0;
          font-size: 12px;
          color: rgb(147,153,159);
        }
      }
    }
  }
</style>
