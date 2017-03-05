<template>
	<div class="cartControl">
		<div class="cart-decrease" v-show="food.count>0" @click='decreaseCart' transition="move">
			<span class="inner icon-remove_circle_outline "></span>
		</div>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add icon-add_circle" @click='addCart'></div>
	</div>
</template>

<script>
	export default{
		props:{
			food:{
				type:Object
			}
		},
		methods:{
			addCart(){
				if(!this.food.count){
					this.$set(this.food,'count',1)
				}else{
					this.food.count++
				}
			},
			decreaseCart(){
				if (this.food.count) {
		            this.food.count--
		          }
		      }
		}
	}
</script>

<style>
	.cartControl{font-size:0;}
	.cartControl .cart-decrease,.cartControl .cart-add{display:inline-block;padding:6px;}
	.cartControl .inner{line-height:24px;font-size:24px;color:rgb(0,160,220);}
	.cartControl .cart-count{display:inline-block;vertical-align:top;width:12px;padding-top:6px;line-height:24px;text-align:center;font-size:10px;color:rgb(147,153,159);}
	.cartControl .cart-add{line-height:24px;font-size:24px;color:rgb(0,160,220);}
</style>