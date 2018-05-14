<template>
  <div class='ratings' ref="rating">
    <div class="rating-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <p class="title">综合评分</p>
          <p class="rankRate">高于周边商家{{seller.rankRate}}%</p>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="time">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <div class="rating-wrapper">
        <rating-select :ratings="ratings" @toggle="toggleContent" @select="selectRating" :only-content="onlyContent"></rating-select>
        <ul>
          <li v-for="rating in ratings" class="rating-item" v-show="needShow(rating.rateType,rating.text)">
            <div class="info clearfix">
              <div class="avatar">
                <img :src="rating.avatar" alt="">
              </div>
              <div class="data">
                <div class="name">{{rating.username}}</div>
                <div class="score">
                  <star :score="rating.score" :size="24"></star>
                  <span class="deliveryTime" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
                </div>
              </div>
              <span class="time">{{rating.rateTime | dateFormat}}</span>
            </div>
            <p class="text">{{rating.text}}</p>
            <div class="support clearfix">
              <span class="icon" :class="rating.rateType===0?'icon-thumb_up':'icon-thumb-down'"></span>
              <div class="recommend">
                <span v-for="item in rating.recommend" class="recommend-item">{{item}}</span>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import Star from "../star/star";
  import Split from "../split/split";
  import RatingSelect from "../rating-select/rating-select";
  import axios from 'axios';
  import BScroll from 'better-scroll'

  const ALL = 2;
  const debug = process.env.NODE_ENV !== 'production';
  export default {
    components: {
      RatingSelect,
      Split,
      Star
    },
    name: 'ratings',
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        ratings: [],
        onlyContent: false,
        selectType: ALL
      }
    },
    filters: {
      dateFormat() {

      }
    },
    created() {
      const url = debug ? '/api/ratings' : 'http://ustbhuangyi.com/sell/api/ratings';
      axios.get(url)
        .then((res) => {
          let result = res.data;
          if (result.code === 0) {
            this.ratings = result.data;
            setTimeout(() => {
              if (!this.scroll) {
                this.scroll = new BScroll(this.$refs.rating, {
                  click: true
                })
              } else {
                this.scroll.refresh();
              }
            }, 20)
          }
        })
    },
    computed: {},
    methods: {
      toggleContent() {
        this.onlyContent = !this.onlyContent;

        setTimeout(() => {
          this.scroll.refresh();
        }, 20)
      },
      selectRating(type) {
        this.selectType = type;
        setTimeout(() => {
          this.scroll.refresh();
        }, 20)
      },
      needShow(type, text) {
        if (this.onlyContent && !text) {
          return false
        }
        if (this.selectType === ALL) {
          return true
        } else {
          return type === this.selectType
        }
      }
    }
  }
</script>

<style lang='scss' scoped>
  .ratings {
    position: absolute;
    left: 0;
    top: 174px;
    bottom: 0;
    width: 100%;
    overflow: hidden;
    .overview {
      padding: 18px 0;
      display: flex;
      .overview-left {
        flex: 0 0 137px;
        width: 137px;
        text-align: center;
        border-right: 1px solid rgba(147, 153, 159, .1);
        .score {
          font-size: 24px;
          color: rgb(255, 153, 0);
          line-height: 28px;
          margin: 0;
        }

        .title {
          margin-top: 6px;
          font-size: 12px;
          color: rgb(7, 17, 27);
          line-height: 12px;
          font-weight: 500;
        }
        .rankRate {
          margin-top: 8px;
          font-size: 10px;
          color: rgb(147, 153, 159);
          line-height: 10px;
        }
      }
      .overview-right {
        flex: 1;
        padding: 0 24px;
        .score-wrapper {
          font-size: 0;
          margin-bottom: 8px;
          .title {
            display: inline-block;
            font-size: 12px;
            color: rgb(7, 17, 27);
            line-height: 18px;
            margin-right: 12px;
            font-weight: 500;
          }
          .star {
            width: 100px;
            display: inline-block;
            vertical-align: top;
          }
          .score {
            display: inline-block;
            margin-left: 12px;
            font-size: 12px;
            color: rgb(255, 153, 0);
            line-height: 18px;
          }
        }
        .delivery-wrapper {
          font-size: 0;
          .title {
            display: inline-block;
            font-size: 12px;
            color: rgb(7, 17, 27);
            line-height: 18px;
            margin-right: 12px;
            font-weight: 500;
          }
          .time {
            font-size: 12px;
            color: rgb(147, 153, 159);
            line-height: 18px;
          }
        }
      }
    }
    .rating-wrapper {
      .rating-select .switch {
        padding-left: 18px;
      }
      .rating-item {
        padding: 18px;
        border-bottom: 1px solid rgba(7, 17, 27, .1);
        .info {
          .avatar {
            float: left;
            width: 28px;
            height: 28px;
            margin-right: 12px;
            img {
              width: 28px;
              height: 28px;
              border-radius: 50%;
            }
          }
          .data {
            float: left;
            .name {
              font-size: 10px;
              color: rgb(7, 17, 27);
              font-weight: 500;
              line-height: 12px;
              margin-bottom: 4px;
            }
            .score {
              font-size: 0;
              .star {
                display: inline-block;
                width: 65px;
                margin-right: 6px;
              }
              .deliveryTime {
                display: inline-block;
                font-size: 10px;
                color: rgb(147, 153, 159);
                line-height: 12px;
              }
            }

          }
        }
        .text {
          margin: 0 0 0 40px;
          font-size: 12px;
          color: rgb(7, 17, 27);
          line-height: 18px;
          font-weight: 500;
        }
        .support {
          margin-top: 8px;
          margin-left: 40px;
          font-size: 0;
          .icon {
            float: left;
            display: block;
            font-size: 12px;
            line-height: 16px;
            margin-right: 8px;
            &.icon-thumb_up {
              color: rgb(0, 160, 220);
            }
            &.icon-thumb-down {
              color: rgb(183, 187, 191);
            }
          }
          .recommend {
            float: left;
            width: 230px;
          }
          .recommend-item {
            display: inline-block;
            text-align: center;
            margin-right: 6px;
            margin-bottom: 6px;
            width: 70px;
            padding: 0 6px;
            box-sizing: border-box;
            height: 16px;
            font-size: 9px;
            color: rgb(147, 153, 159);
            line-height: 16px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            border: 1px solid rgba(7, 17, 27, .1);
            border-radius: 1px;
          }
        }
      }
    }
  }
</style>
