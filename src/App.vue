<template>
  <div id="app">
   
      <v-header :seller="seller"></v-header>
      <div class="tab">
        <div class="tab-item" @click="changeTab('goods')">
          <router-link to="/goods" :class="{red:curtab=='goods'}" >商品</router-link>
        </div>
        <div class="tab-item"  @click="changeTab('ratings')">
          <router-link to="/ratings" :class="{red:curtab=='ratings'}">评论</router-link>
        </div>
        <div class="tab-item" @click="changeTab('seller')">
          <router-link to="/seller" :class="{red:curtab=='seller'}" >商家</router-link>
        </div>
      </div>
    <div class="main">
      <keep-alive>
        <router-view :seller="seller">
        </router-view>
      </keep-alive>
    </div>
  </div>
</template>
<style>
a{
  text-decoration: none;
  color:#000;
}
.a{
  height:50px;
  border:1px solid #000;
}
.tab{
  display:flex;
  height:30px;
  line-height:30px;
  font-size:14px;
  border-bottom:1px solid #ddd;
}
.tab .tab-item{
  flex:1;
  text-align:center;
}
.red{
  color:red;
}
.fixed{
  z-index:1000;
  position: fixed;
  top:0;
  width:100%;
  background-color:#fff;
} 
.main{
  margin-top:172px;
} 
</style>
<script>
let ERR_OK=0;
import Header from '@/components/header/header'
const response = require('./common/data/seller.json');
  export default {
    data() {
      return {
        // seller: {
        //   id: (() => {
        //     let queryParam = urlParse();
        //     return queryParam.id;
        //   })()
        // },
        curtab:'goods'
      }
    },
    methods:{
      changeTab:function(item){
        this.curtab=item;
      }
    },
    created() {
      // this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      //   response = response.body;
      //   if (response.errno === ERR_OK) {
      //     this.seller = Object.assign({}, this.seller, response.data);
      //   }
      // });
      if (response.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, response.data);
      }
      // console.log(this.seller);
    },
    components: {
      'v-header': Header
    }
  }
</script>

