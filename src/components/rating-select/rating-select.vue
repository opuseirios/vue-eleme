<template>
  <div class='rating-select'>
    <div class="rating-type border-1px">
      <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">{{desc.all}}<span
        class="count">{{ratings.length}}</span></span>
      <span @click="select(0,$event)" class="block positive" :class="{'active':selectType===0}">{{desc.positive}}<span
        class="count">{{positives.length}}</span></span>
      <span @click="select(1,$event)" class="block negative" :class="{'active':selectType===1}">{{desc.negative}}<span
        class="count">{{negatives.length}}</span></span>
    </div>
    <div class="switch">
      <span @click="toggleContent" class="icon-check_circle" :class="{'on':onlyContent}"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
  const ALL = 2;
  const POSITIVE = 0;
  const NEGATIVE = 1;
  export default {
    name: 'rating-select',
    props: {
      ratings: {
        type: Array,
        default() {
          return [];
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default() {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          }
        }
      }
    },
    computed: {
      positives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        });
      },
      negatives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE;
        });
      }
    },
    methods: {
      select(type, event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('select', type);
      },
      toggleContent(event){
        if (!event._constructed) {
          return;
        }
        this.$emit('toggle');
      }
    }
  }
</script>

<style lang='scss' scoped>
  @import "./../../assets/scss/mixin";

  .rating-select {
    .rating-type {
      font-size: 0;
      padding: 18px;
      @include border-1px(rgba(7, 17, 27, .1));
      .block {
        display: inline-block;
        width: 60px;
        height: 32px;
        margin-right: 8px;
        text-align: center;
        line-height: 32px;
        border-radius: 2px;
        font-size: 12px;
        color: rgb(77, 85, 93);
        &.active {
          color: #fff;
        }
        &.positive {
          background: rgba(0, 160, 220, .2);
          &.active {
            background: rgb(0, 160, 220);
          }
        }
        &.negative {
          background-color: rgba(77, 85, 93, .2);
          &.active {
            background-color: rgb(77, 85, 93);
          }
        }
      }
    }
    .switch {
      padding: 12px 0;
      border-bottom: 1px solid rgba(147, 153, 159, .2);
      font-size: 0;
      .icon-check_circle {
        display: inline-block;
        font-size: 24px;
        line-height: 24px;
        color: rgb(147, 153, 159);
        &.on {
          color: rgb(0, 160, 220);
        }
      }
      .text {
        vertical-align: top;
        display: inline-block;
        margin-left: 4px;
        font-size: 12px;
        line-height: 24px;
        color: rgb(147, 153, 159);
      }
    }
  }
</style>
