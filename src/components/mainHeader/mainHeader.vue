<template>
  <div class="mainHeader">
    <div class="container">
      <div class="avatar">
        <img :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="desc">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="supports" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
        <div class="support-count" v-if="seller.supports" @click="show">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
    </div>
    <div class="bulletin" @click="show">
      <span class="bulletin-icon"></span>
      <p class="text">{{seller.bulletin}}</p>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <!--背景模糊效果-->
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%" alt="">
    </div>
    <!--详情页-->
    <transition name="fade">
    <div class="detail" v-show="showDetail">
      <div class="detail-wrapper">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :score="seller.score" :size="48"></star>
          </div>
          <!--优惠信息-->
          <div class="info">
            <txt :txt="txt1"></txt>
            <ul>
              <li v-for="item in seller.supports" class="support">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="desc">{{item.description}}</span>
              </li>
            </ul>
          </div>
          <!--商家公告-->
          <div class="bulletin-wrapper">
            <txt :txt="txt2"></txt>
            <div class="bulletin">{{seller.bulletin}}</div>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hide">
        <i class="icon-close"></i>
      </div>
    </div>
    </transition>
  </div>
</template>

<script>
  import Star from "../star/star";
  import Txt from "../txt/txt";

  export default {
    components: {
      Txt:Txt,
      Star
    },
    name: 'main-header',
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        txt1: '优惠信息',
        txt2: '商家公告',
        showDetail: false
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
    },
    methods: {
      hide() {
        this.showDetail = false
      },
      show() {
        this.showDetail = true;
      }
    }
  }
</script>

<style lang='scss' scoped>
  @import "./../../assets/scss/mixin";

  .mainHeader {
    width: 100%;
    background: rgba(7, 17, 27, .5);
    color: #fff;
    position: relative;
    overflow: hidden;
    .container {
      position: relative;
      width: 100%;
      font-size: 0;
      .avatar {
        display: inline-block;
        padding: 24px 16px 18px 24px;
        width: 64px;
        height: 64px;
        img {
          border-radius: 2px;
          width: 100%;
          height: 100%;
        }
      }
      .content {
        padding-top: 24px;
        display: inline-block;
        vertical-align: top;
        .title {
          padding-top: 2px;
          .brand {
            display: inline-block;
            width: 30px;
            height: 18px;
            margin-right: 6px;
            background-size: 30px 18px;
            @include bg-image('brand');
          }
          .name {
            vertical-align: top;
            display: inline-block;
            font-size: 16px;
            font-weight: bold;
            line-height: 18px;
          }
        }
        .desc {
          margin-top: 8px;
          font-size: 12px;
          color: #fff;
          line-height: 12px;
        }
        .supports {
          margin-top: 10px;
          .icon {
            display: inline-block;
            width: 12px;
            height: 12px;
            background-size: 12px 12px;
            &.discount {
              @include bg-image('discount_1')
            }
            &.decrease {
              @include bg-image('decrease_1')
            }
            &.guarantee {
              @include bg-image('guarantee_1')
            }
            &.invoice {
              @include bg-image('invoice_1')
            }
            &.special {
              @include bg-image('special_1')
            }
          }
          .text {
            vertical-align: top;
            display: inline-block;
            margin-left: 4px;
            font-size: 10px;
            line-height: 12px;
          }
        }
        .support-count {
          position: absolute;
          right: 12px;
          bottom: 20px;
          border-radius: 10px;
          padding: 6px 8px;
          font-size: 0;
          background-color: rgba(0, 0, 0, .2);
          .count {
            display: inline-block;
            font-size: 10px;
            line-height: 12px;
          }
          .icon-keyboard_arrow_right {
            display: inline-block;
            margin-left: 2px;
            vertical-align: top;
            line-height: 12px;
            font-size: 10px;
          }
        }
      }
    }
    /*公告*/
    .bulletin {
      width: 100%;
      position: relative;
      padding: 0 12px;
      box-sizing: border-box;
      background-color: rgba(7, 17, 27, .2);
      font-size: 0;
      height: 28px;
      line-height: 28px;
      .bulletin-icon {
        vertical-align: middle;
        display: inline-block;
        width: 22px;
        height: 12px;
        background-size: 22px 12px;
        @include bg-image('bulletin')
      }
      .text {
        vertical-align: middle;
        padding-left: 8px;
        display: inline-block;
        width: 88%;
        margin: 0;
        font-size: 10px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
      .icon-keyboard_arrow_right {
        position: absolute;
        font-size: 10px;
        right: 12px;
        top: 8px;
      }
    }
    /*背景图片的模糊效果*/
    .background {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      filter: blur(10px);
    }
    /*详情页*/
    .detail {
      position: fixed;
      z-index: 100;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      -webkit-backdrop-filter: blur(10px);
      opacity: 1;
      background: rgba(7, 17, 27, .8);
      overflow: auto;
      &.fade-enter-active,&.fade-leave-active{
        transition: all .5s;
      }
      &.fade-enter,&.fade-leave-to{
        opacity: 0;
      }
      .detail-wrapper {
        width: 100%;
        min-height: 100%;
        .detail-main {
          padding-top: 64px;
          padding-bottom: 64px;
          .name {
            text-align: center;
            font-size: 16px;
            font-weight: 700;
            line-height: 16px;
          }
          .star-wrapper {
            margin-top: 16px;
            text-align: center;
          }
          .info {
            margin: 0 36px;
            ul {
              list-style: none;
              margin-top: 24px;
              padding: 0 12px;
              .support {
                font-size: 0;
                margin-bottom: 12px;
                &:last-child {
                  margin-bottom: 0;
                }
                .icon {
                  display: inline-block;
                  margin-right: 6px;
                  width: 16px;
                  height: 16px;
                  background-size: 16px 16px;
                  background-repeat: no-repeat;
                  &.discount {
                    @include bg-image('discount_1')
                  }
                  &.decrease {
                    @include bg-image('decrease_1')
                  }
                  &.guarantee {
                    @include bg-image('guarantee_1')
                  }
                  &.invoice {
                    @include bg-image('invoice_1')
                  }
                  &.special {
                    @include bg-image('special_1')
                  }
                }
                .desc {
                  vertical-align: top;
                  margin-top: 2px;
                  display: inline-block;
                  font-size: 12px;
                  line-height: 12px;
                }
              }
            }
          }
          .bulletin-wrapper {
            margin: 0 36px;
            .bulletin {
              padding: 0 12px;
              font-size: 12px;
              line-height: 24px;
            }
          }
        }
      }
      .detail-close {
        position: relative;
        width: 32px;
        height: 32px;
        margin: -64px auto 0;
        clear: both;
        .icon-close {
          font-size: 32px;
          color: rgba(255, 255, 255, .5);
        }
      }
    }
  }
</style>
