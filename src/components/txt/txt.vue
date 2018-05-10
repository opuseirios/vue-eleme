<template>
  <div class='title'>
    {{txt}}
  </div>
</template>

<script>
  export default {
    name: '',
    props: {
      txt: {
        type: String
      }
    }
  }
</script>

<style lang='scss' scoped>
  .title {

    box-sizing: border-box;
    margin: 28px 0 24px 0;
    font-size: 14px;
    font-weight: 700;
    line-height: 14px;
    text-align: center;
    position: relative;
    &::before {
      position: absolute;
      left: 0;
      content: '';
      width: 112px;
      height: 1px;
      background: rgba(255, 255, 255, .2);
      top: 50%;
    }
    &::after {
      position: absolute;
      content: '';
      width: 112px;
      height: 1px;
      background: rgba(255, 255, 255, .2);
      top: 50%;
      right: 0;
    }
  }
</style>
