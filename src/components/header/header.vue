<template>
  <div class="header">
    <div class="content-wrap">
      <div class="avatar fl">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content fl">
        <div class="brand">
          <span class="Img-brand"></span>
          <span>{{seller.name}}</span>
        </div>
        <div class="description f12">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
        <div v-if="seller.supports" class="support f12">
          <i class="Img-jian"></i>
          <span>{{seller.supports[0].description}}</span>
          <span class="support-count fr" @click="showDetail">
            <span>{{seller.supports.length}}个</span> 
            <i class="icon-keyboard_arrow_right"></i>   
          </span>
        
        </div>
      </div>
    </div>
    <div class="bulletin-wrap" @click="showDetail">
      <span class="Img-ad"></span>
      <span >{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i> 
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <!-- <star :size="48" :score="seller.score"></star> -->
            </div>
            <div class="title">
              <div class="text text-line">优惠信息</div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="text text-line">商家公告</div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <span class="icon-close"></span>
        </div>
      </div>
    </transition>
  </div>
  
</template>
<style scoped>
.Img-ad,
.Img-brand,
.Img-jian{
  display: inline-block;
  vertical-align: top;
}
.decrease,
.discount,
.special,
.invoice,
.guarantee{
  display: inline-block;
  vertical-align: top;
  margin-top:8px;
  width: 16px;
  height: 16px;
  background-size:16px 16px !important;
}

.decrease{
  background:url('./decrease_1@2x.png') no-repeat;
} 
.discount{
  background:url('./discount_1@2x.png') no-repeat;
} 
.special{
  background:url('./special_1@2x.png') no-repeat;
}
.invoice{
  background:url('./invoice_1@2x.png') no-repeat;
}
.guarantee{
  background:url('./guarantee_1@2x.png') no-repeat;
}
.text-line{
  text-align:center;
  margin-bottom:30px;
}
.text-line:before,.text-line:after{
  position:absolute;
  top:6px;
  content:'';
  display:block;
  width:calc(50% - 50px);
  height:2px;
  background-color:rgba(255,255,255,0.2);
}
.text-line:before{
  left:0;
}
.text-line:after{
  right:0;
}
.icon-keyboard_arrow_right{
  line-height:24px;
  margin-left:-2px;
}
.Img-brand{
    width: 30px;
    height: 18px;
    background:url('./brand@2x.png') no-repeat;
    background-size: 30px 18px;
}
.Img-ad{
  width:20px;
  height:12px;
  margin-top:8px;
  background:url('./bulletin@2x.png') no-repeat;
  background-size: 20px 12px;
}
.Img-jian{
  display:inline-block;
  vertical-align:top;
  width:12px;
  height:12px;
  background:url('./decrease_1@2x.png') no-repeat;
  background-size: 12px 12px;
}
.line{
  border-bottom:1px solid #fff;
}
.text{
  position: relative;
  top:10px;
  padding:0 10px;
}
.icon-close{
  font-size:44px;

}
.detail-close{
  position:absolute;
  bottom:24px;
  left:0;
  width:100%;
  text-align:center;
}  
a{
  text-decoration: none;
  color:#000;
}
.bg-brand,.bg-jie,.bg-ad{
  display:inline-block;
  padding:0 4px;
  border-radius:2px;
}
.bg-brand{
  height:18px;
  line-height:18px;
  color:#fff;
  background-color:red;
  font-size:12px;
}
.f12{
  font-size:12px;
}
.header{
  color:#fff;
  background-color:rgba(7, 17, 27, 0.5);
}
.content-wrap{
  position:relative;
  padding:24px 18px;
  overflow: hidden;
}
.avatar{
  width:84px;
  height:64px;
}
.content{
  width:calc(100% - 84px);
}
.description{
  margin:8px 0 10px;
}
.support-count{
  display:inline-block;
  width:44px;
  height:24px;
  line-height:24px;
  border-radius:24px;
  background-color: rgba(7,17,27,0.2);
  text-align:center;
}
.bulletin-wrap{
  position:relative;
  padding:0 10px;
  height:28px;
  line-height:28px;
  font-size:12px;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  background-color: rgba(7,17,27,0.2);
}   
.bulletin-wrap .icon-keyboard_arrow_right{
  position: absolute;
  right:12px;
  top:2px;
}
.detail{
  position:fixed;
  top:0;
  left:0;
  width:100%;
  height:100%;
  z-index:100;
  background-color:rgba(7,0,7,.7);
}
.detail-wrapper{
  width:80%;
  margin:0 auto;
}
.detail-wrapper .name{
    line-height: 16px;
    text-align: center;
    font-size: 16px;
    font-weight: 700;
    margin-top:60px;
}
.detail .bulletin{
  line-height:24px;
  font-size: 12px;
}
.detail .support-item{
  margin-bottom:12px;
  font-size:12px;
}
</style>
<script>
 export default {
   props:{
    seller: {
      type: Object
    }
   },
   data(){
     return{
        detailShow:false
     }
   },
   methods:{
        showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
   },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
 }
</script>
