<template>
  <div id="app">
    <main-header :seller="seller"></main-header>
    <div class="tabs">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评分</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view/>
  </div>
</template>
<script>
import MainHeader from "./components/mainHeader/mainHeader";

import axios from 'axios';
export default {
  components: {MainHeader},
  name: 'App',
  data(){
    return{
      seller:{}
    }
  },

  mounted(){
    /*获取data.json里seller数据*/
    axios.get('/api/seller').then((res)=>{
       let result = res.data;
       if(result.code===0){
         this.seller = result.data;
       }
    })
  }
}
</script>

<style lang="scss">
  @import "assets/scss/mixin";
  body{
    margin: 0;
  }
#app {
  .tabs{
    width: 100%;
    display: flex;
    height: 40px;
    line-height: 40px;
    @include border-1px(rgba(7,17,27,.1));
    .tab-item{
      flex: 1;
      display: inline-block;
      text-align: center;
      a{
        display: block;
        width: 100%;
        height: 100%;
        font-weight: 500;
        text-decoration: none;
        color: rgb(77,85,93);
        font-size: 14px;
        &.router-link-active{
          color: rgb(240,20,20);
        }
      }
    }
  }
}
</style>
