<template>
  <div class='ratings'>
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
    </div>
  </div>
</template>

<script>
  import Star from "../star/star";

  export default {
    components: {Star},
    name: 'ratings',
    props: {
      seller: {
        type: Object
      }
    },

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
    .overview{
      padding: 18px 0;
      display: flex;
      .overview-left{
        flex: 0 0 137px;
        width: 137px;
        padding: 6px 0;
        text-align: center;
        border-right: 1px solid rgba(147,153,159,.1);
        .score{
          font-size: 24px;
          color: rgb(255,153,0);
          line-height: 28px;
        }

        .title{
          margin-top: 6px;
          font-size: 12px;
          color: rgb(7,17,27);
          line-height: 12px;
        }
        .rankRate{
          margin-top: 8px;
          font-size: 10px;
          color: rgb(147,153,159);
          line-height: 10px;
        }
      }
      .overview-right{
        flex: 1;
        padding: 0 24px;
        .score-wrapper{
          font-size: 0;
          .title{
            display: inline-block;
            font-size: 12px;
            color: rgb(7,17,27);
            line-height: 18px;
            margin-right: 12px;
          }
          star{
            width: 200px;
            display: inline-block;
          }
          .score{
            display: inline-block;
            margin-left: 12px;
            font-size: 12px;
            color: rgb(255,153,0);
            line-height: 18px;
          }
        }
      }
    }
  }
</style>
