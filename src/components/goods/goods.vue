<template>
    <div>
        <div class="goods">
            <div class="menu-wrapper" ref="menuWrap">
                <ul>
                    <li v-for="(item,index) in goods"  :class="{'bg-fff':currentIndex===index}" class="menu-item"  @click="selectMenu(index)">
                        <div class="text">
                            <!-- <span v-show="item.type">{{item.type}}</span> -->
                            <span>{{item.name}}</span>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="food-wrapper" ref="foodWrap">
                <ul>
                    <li class='food-list' v-for="item in goods" ref="foodList">
                        <div class="title">
                            {{item.name}}
                        </div>
                        <ul v-for="food in item.foods" >
                            <li class="food-item">
                                <div class="pic">
                                    <img :src="food.icon" alt="">
                                </div>
                                <div class="content">
                                    <div class="fl">
                                        <div class="name">
                                            {{food.name}}
                                        </div>
                                        <div class="desc">
                                            {{food.description}}
                                        </div>
                                        <div class="extra">
                                            <span style="margin-right:8px;">月售： {{food.sellCount}}份</span>
                                            <span>好评率：{{food.rating}}%</span> 
                                        </div>
                                        <div class="price red">
                                            ￥{{food.price}}
                                        </div>
                                    </div>
                                </div>
                                <div class="cartcontrol-wrapper">
                                    <cartcontrol @add="addFood" :food="food"></cartcontrol>
                                    <!-- <cartcontrol @add="addFood" :food="food"></cartcontrol> -->
                                </div>
                            </li>
                        </ul>
                    </li>
                </ul>
            </div>
        </div>
        <shopcart :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice"
                :minPrice="seller.minPrice"></shopcart>
    </div>
</template>
<script>
import BScroll from 'better-scroll'
import shopcart from '@/components/shopcart/shopcart'
import cartcontrol from '@/components/cartcontrol/cartcontrol'
const response=require('@/common/data/goods.json');
export default {
     props: {
      seller: {
        type: Object
      }
    },
    data(){
        return{
            goods:[],
            scrollY:0,
            listHeight:[]
        }
    },
    components:{
      shopcart,
      cartcontrol
    },
    created(){
        this.goods=response.data;
        this.$nextTick(()=>{
            this.init();
            this.calculateHeight();
        })
    },
    computed: {
        currentIndex() {
            for (let i = 0; i < this.listHeight.length; i++) {
            let height1 = this.listHeight[i];
            let height2 = this.listHeight[i + 1];
            if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                return i;
            }
            }
            return 0;
        },
        selectFoods() {
            let foods = [];
            this.goods.forEach((good) => {
            good.foods.forEach((food) => {
                if (food.count) {
                foods.push(food);
                }
            });
            });
            return foods;
        }
    },
    methods:{
        init:function(){
           this.menuScroll=new BScroll(this.$refs.menuWrap,{
            click:true
           });
           this.foodScroll=new BScroll(this.$refs.foodWrap,{
                click: true,
                probeType: 3
           });
            this.foodScroll.on('scroll', (pos) => {
                this.scrollY = Math.abs(Math.round(pos.y));
            });
        },
        calculateHeight:function(){
            let foodList = this.$refs.foodList;
            let height = 0;
            this.listHeight.push(height);
            for (let i = 0; i < foodList.length; i++) {
            let item = foodList[i];
            height += item.clientHeight;
            this.listHeight.push(height);
            }
        },
        selectMenu:function(index,event){
            console.log(index);
            let foodList = this.$refs.foodList;
            let el=foodList[index];
            this.foodScroll.scrollToElement(el,300);
        },
        addFood(target) {
            this._drop(target);
        },
        _drop(target) {
        // 体验优化,异步执行下落动画
            this.$nextTick(() => {
            this.$refs.shopcart.drop(target);
            });
        }
    }
}
</script>

<style>
.bg-fff{
    background-color:#fff !important;
}
    .goods{
        display:flex;
        position:absolute;
        top:179px;
        bottom:46px;
        width:100%;
    }
    .menu-wrapper{
        flex:0 0 80px;
        width:80px;
        overflow:hidden;
        background-color:#f3f5f7;
    }
    .food-wrapper{
        flex:1;
        overflow: hidden;
    }
    .food-list .title{
        height:26px;
        line-height:26px;
        padding-left:14px;
        font-size:12px;
        color:rgb(147,153,159);
        background-color:#f4f5f7;
        border-left:2px solid #d9dde1;
    }
    .food-item{
        position:relative;
        display: flex;
        padding:10px 0 10px 18px;
        border-bottom:1px solid #ddd;
    }
    .food-item .cartcontrol-wrapper{
        position:absolute;
        right:10px;
        bottom:10px;
        /* width:30px;
        height:30px; */
    }
    .food-item .pic{
        flex:0 0 54px;
        margin-right:10px;
    }
     .food-item .pic img{
        width:100%;
    }
    .food-item .content{
        flex:1;
        padding-right: 20px;
    }
    .food-item .name{
        margin:2px 0 8px;
        height:14px;
        line-height:14px;
        font-size:14px;
        color:rgb(7,17,27);
    }
    .food-item .desc,.food-item .extra{
        color: #93999f;
        font-size:12px;
    }
    .food-item  .desc{
        margin-bottom:8px;
        line-height:10px;
    }
    .food-item .extra{
        line-height:12px;
        margin-bottom:8px;
    }
    .menu-item{
        display:table;
        width:56px;
        height:54px;
        padding:0 12px;
        line-height:14px;
        border-bottom:1px solid #ddd;
    }
    .menu-item .text{
        display:table-cell;
        vertical-align: middle;
        width:56px;
        font-size:12px;
    }
</style>