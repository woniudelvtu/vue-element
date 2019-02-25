<template>
    <div class="shopcart">
         <div class="cart-bar">
            <div class="fl">
                <span class="logo fl" :class="{active:totalCount!=0}" @click="changeShow">
                    <i class="icon-shopping_cart" ></i>
                    <span class="num" v-show="totalCount>0">{{totalCount}}</span>
                </span>
                <span class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</span>
                <span class="delivery">另需配送费￥{{deliveryPrice}}元</span>
            </div>
            <div class="fr sendPrice" :class="payClass" @click.stop.prevent="pay">
               {{payDesc}}
            </div>
        </div>
     
        <div class="cart-list" v-if="show">
            <div class="title">
                <span class="fl">购物车</span>
                <span class="fr primary" @click="empty">清空</span>
            </div>
            <ul class="content">
                <li v-for="item in  selectFoods" class="cart-item">
                <span>{{item.name}}</span> 
                    <span class="cartcontrol-wrapper fr">
                        <cartcontrol @add="addFood" :food="item"></cartcontrol>
                    </span>
                    <span class="red fr" style="margin-right:10px;">￥{{item.count*item.price}}</span>
                </li>
            </ul>
        </div>
        <div class="modal-wrap" v-if="show">
        </div>
    </div>
</template>
<style scoped>
    .shopcart{
        z-index:50;
        position:fixed;
        left:0;
        bottom:0;
        box-sizing : border-box;
        width:100%;
        height:56px;
        line-height:56px;
        font-size:12px;
        color: rgba(255,255,255,0.4);
        background-color:#141d27;
    }
    .cart-bar{
        padding-left:20px;
    }
    .list-header{
        height: 40px;
        line-height: 40px;
        padding: 0 18px;
        background: #f3f5f7;
        border-bottom: 1px solid rgba(7,17,27,0.1);
    }
    .logo{
        display:inline-block;
        position: relative;
        top:-8px;
        width:44px;
        height:44px;
        border-radius:50%;
        text-align:center;
        border:10px solid #141d27;
        background-color:#2b343c;
    }
    .logo .num{
        position:absolute;
        right:-6px;
        top:-8px;
        width:20px;
        height:20px;
        line-height:20px;
        border-radius:50%;
        text-align:center;
        color:#fff;
        background-color:red;
    }
    .active{
        background-color:#00a0dc !important;
    }
    .total{
        font-weight: 700;
        font-size:16px;
        margin:0 6px 0 10px;
    }
    .icon-shopping_cart{
        color:#fff;
    }
    .cart{
        position:absolute;
        width:100%;
        max-height:200px;
        overflow:scroll;
        bottom:56px;
        color:#000;
        background-color: #fff;
    }
    .cart li{
        height:40px;
    }
    .list-content{
        color:#000;
        font-size:14px;
        padding:0 20px;
    }
    .list-item{
        height:40px;
        line-height:40px;
        overflow: hidden;
        border-bottom:1px solid #ddd;
    }
    .price{
        margin-right:6px;
        font-weight: 700;
        font-size:16px;
    }
    .delivery{
        margin-left:14px;
        padding-left:14px;
        border-left:1px solid rgba(255,255,255,0.4);
    }
    .sendPrice{
        width:125px;
        text-align: center;
    }
    .not-enough{
        background: #2b333b
    }       
    .enough{
        background: #00b43c;
        color: #fff;
    }
    .cart-list{
        z-index:45;
        position: absolute;
        bottom:54px;
        left:0;
        width:100%;
        max-height:260px;
        overflow: scroll;
        color:#000;
        background-color:#fff;
    }  
    .cart-list .title{
        height: 40px;
        line-height: 40px;
        padding:0 10px;
        overflow: hidden;
        background: #f3f5f7;
        color:#000;
        border-bottom: 1px solid rgba(7,17,27,0.1);
    } 
    .cart-list .content{
        padding:0 10px;
    }
    .cart-list .cart-item{
        height:30px;
        line-height:30px;
        padding:10px;
        border-bottom:1px solid #ddd;
    }
     .cart-list .cart-item:last-child{
          border-bottom:none;
     }
    .cartcontrol-wrapper{
        margin-top:-4px;
    }
    .modal-wrap{
        z-index:40;
        position:fixed;
        top:0;
        left:0;
        width:100%;
        height:100%;
        background: rgba(7,17,27,0.6);
    }
</style>
<script>
import cartcontrol from '@/components/cartcontrol/cartcontrol'
    export default {
        props: {
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            },
            selectFoods: {
                type: Array
            },
            total:{
               type: Number,
                default: 0  
            },
            cart:{
                type:Array
            },
            show:false
        },
        data(){
            return {
                show:false
            }
        },
        components:{
            cartcontrol
        },
        methods:{
            empty:function(){
                this.cart=[];
            },
            changeShow:function(){
                if(this.selectFoods.length>0){
                    this.show=!this.show;
                }
               
            },
            pay:function(){
                let money=Number(this.totalPrice)+Number(this.deliveryPrice);
                alert("欢迎您支付"+ money+"成功");
            },
            addFood(target) {
                this.drop(target);
            },
            empty:function(){
                this.selectFoods.forEach((food) => {
                    food.count = 0;
                });
            }
        },
        computed:{
            totalPrice() {
                let total = 0;
                this.selectFoods.forEach((food) => {
                    total += food.price * food.count;
                });
                return total;
            },
            totalCount() {
                let count = 0;
                this.selectFoods.forEach((food) => {
                count += food.count;
                });
                return count;
            },
            payDesc() {
                if (this.totalPrice === 0) {
                    return `￥${this.minPrice}元起送`;
                } 
                else if (this.totalPrice < this.minPrice) {
                    let diff = this.minPrice - this.totalPrice;
                    return `还差￥${diff}元起送`;
                } 
                else {
                    return '去结算';
                }
            },
            payClass() {
                if (this.totalPrice < this.minPrice) {
                return 'not-enough';
                } else {
                return 'enough';
                }
            }
        }
    }
</script>