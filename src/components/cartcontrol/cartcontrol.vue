<template>
    <div class="count-wrap">
        <span class="cart-decrease inner icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart"></span>
        <span class="cart-count"  v-show="food.count>0">{{food.count}}</span>
        <span class="cart-add icon-add_circle" @click="addCart"></span>
    </div>

</template>
<script>
import Vue from 'vue';
export default {
    props:{
        food:{
           type:Array
        }
    //     cart:{
    //         type:Array
    //     }
    },
    methods:{
        addCart:function(event){
            if (!this.food.count) {
                Vue.set(this.food, 'count', 1);
            } 
            else {
                this.food.count++;
            }
            this.$emit('add', event.target);
        },
        decreaseCart:function() {
        // if (!event._constructed) {
        //     return;
        // }
            if (this.food.count) {
                this.food.count--;
            }
        }
    }
}
</script>
<style>
.count-wrap{
    color:#000;
}
    .count{
        margin:0 6px;
    }
</style>
