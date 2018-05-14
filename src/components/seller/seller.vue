<template>
  <div class='seller' ref="seller">
    <div>
      <div class="header border-1px">
        <div class="name">{{seller.name}}</div>
        <div class="star-wrapper">
          <star :size="36" :score="seller.score"></star>
          <span class="ratingCount">({{seller.ratingCount}})</span>
          <span class="count">月售{{seller.sellCount}}单</span>
        </div>
      </div>
      <div class="delivery-info">
        <div class="block">
          <span class="title">起送价</span>
          <div class="num"><span class="count">{{seller.minPrice}}</span>元</div>
        </div>
        <div class="block">
          <span class="title">商家配送</span>
          <div class="num"><span class="count">{{seller.deliveryPrice}}</span>元</div>
        </div>
        <div class="block">
          <span class="title">平均配送时间</span>
          <div class="num"><span class="count">{{seller.deliveryTime}}</span>分钟</div>
        </div>
      </div>
      <split></split>
      <div class="bulletin-wrapper">
        <div class="title">公告与活动</div>
        <div class="text">{{seller.bulletin}}</div>
        <ul>
          <li v-for="support in seller.supports" class="support">
            <span class="icon" :class="classMap[support.type]"></span>
            <span class="text">{{support.description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics" ref="pics">
        <div class="title">商家实景</div>
        <div class="pic-wrapper " ref="picWrapper">
          <ul class="pic-list clearfix" ref="picList">
            <li v-for="pic in seller.pics" class="pic pic-hook">
              <img :src="pic" alt="">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="infos">
        <div class="title">商家信息</div>
        <div class="info" v-for="info in seller.infos">{{info}}</div>
      </div>
    </div>
  </div>
</template>

<script>
  import Star from "../star/star";
  import Split from "../split/split";
  import BScroll from 'better-scroll'

  const picWidth = 120;
  const margin = 6;

  export default {
    components: {
      Split,
      Star
    },
    name: 'seller',
    props: {
      seller: {
        type: Object
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];

    },
    mounted() {
      setTimeout(() => {
        this._initScroll();
        this._initPicScroll()
      }, 20);
    },
    methods: {
      _initScroll() {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          })
        } else {
          this.scroll.refresh();
        }
      },
      _initPicScroll() {
        if (this.seller.pics) {
          /*计算ul宽度*/
          let count = this.$refs.picWrapper.getElementsByClassName('pic-hook').length;
          let picListwidth = (picWidth + margin) * count - margin;
          this.$refs.picList.style.width = picListwidth + 'px';
          setTimeout(() => {
            if (!this.picScroll) {
              this.picScroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true,
                eventPassthrough: 'vertical'
              })
            } else {
              this.picScroll.refresh();
            }
          }, 20);
        }
      }
    }
  }
</script>

<style lang='scss' scoped>
  @import "./../../assets/scss/mixin";

  .seller {
    position: absolute;
    left: 0;
    top: 174px;
    bottom: 0;
    width: 100%;
    overflow: hidden;
    .header {
      margin: 0 18px;
      padding: 18px 0;
      @include border-1px(rgba(7, 17, 27, .1));
      .name {
        font-size: 14px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        font-weight: 500;
        margin-bottom: 8px;
      }
      .star-wrapper {
        font-size: 0;
        .star {
          display: inline-block;
          width: 100px;
          vertical-align: top;
          margin-right: 8px;
        }
        .ratingCount {
          font-size: 10px;
          line-height: 18px;
          color: rgb(77, 85, 93);
          font-weight: 500;
          margin-right: 12px;
        }
        .count {
          font-size: 10px;
          line-height: 18px;
          color: rgb(77, 85, 93);
          font-weight: 500;
        }
      }
    }
    .delivery-info {
      padding: 18px 0;
      display: flex;
      .block {
        flex: 1;
        text-align: center;
        border-right: 1px solid rgba(7, 17, 27, .1);
        &:last-child {
          border-right: none;
        }
        .title {
          font-size: 10px;
          color: rgb(147, 153, 159);
          line-height: 10px;
          margin-bottom: 4px;
        }
        .num {
          font-size: 10px;
          color: rgb(7, 17, 27);
          line-height: 24px;
          .count {
            font-size: 24px;
            font-weight: 500;
          }
        }
      }
    }
    .bulletin-wrapper {
      padding: 18px 18px 0 18px;
      .title {
        font-size: 14px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        font-weight: 500;
        margin-bottom: 8px;
      }
      .text {
        padding: 0 12px 16px;
        font-size: 12px;
        color: rgb(240, 20, 20);
        line-height: 24px;
        @include border-1px(rgba(7, 17, 27, .1));
      }
      .support {
        padding: 16px 12px;
        font-size: 0;
        @include border-1px(rgba(7, 17, 27, .1));
        .icon {
          display: inline-block;
          vertical-align: top;
          width: 16px;
          height: 16px;
          margin-right: 6px;
          background-size: 16px 16px;
          &.discount {
            @include bg-image('discount_4')
          }
          &.decrease {
            @include bg-image('decrease_4')
          }
          &.guarantee {
            @include bg-image('guarantee_4')
          }
          &.invoice {
            @include bg-image('invoice_4')
          }
          &.special {
            @include bg-image('special_4')
          }
        }
        .text {
          font-size: 12px;
          color: rgb(7, 17, 27);
          line-height: 16px;
          @include border-none();
        }
      }
    }
    .pics {
      padding: 18px 0 18px 18px;
      .title {
        font-size: 14px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        font-weight: 500;
        margin-bottom: 8px;
      }
      .pic-wrapper {
        width: 100%;
        overflow: hidden;
        white-space: normal;
        .pic-list{
          font-size: 0;
        }
        .pic {
          float: left;
          width: 120px;
          height: 90px;
          margin-right: 6px;
          &:last-child {
            margin-right: 0;
          }
          img {
            width: 120px;
            height: 90px;
          }
        }
      }
    }
    .infos {
      padding: 18px;
      .title {
        font-size: 14px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        font-weight: 500;
        padding-bottom: 12px;
        @include border-1px(rgba(7, 17, 27, .1));
      }
      .info {
        font-size: 12px;
        color: rgb(7, 17, 27);
        line-height: 16px;
        padding: 16px 12px;
        @include border-1px(rgba(7, 17, 27, .1));
      }
    }
  }
</style>
