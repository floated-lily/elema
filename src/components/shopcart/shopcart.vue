<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper" >
					<div class="logo" :class="{'hightlight':totalCount >0}">
						<i class="icon-shopping_cart" :class="{'hightlight':totalCount >0}"></i>
					</div>
					<div class="num" v-show="totalCount>0">
			            {{totalCount}}
			        </div>
				</div>
				<div class="price" :class="{'hightlight':totalCount >0}">${{totalPrice}}元</div>
				<div class="desc">另需配送费${{deliveryPrice}}元</div>
			</div>
			<div class="content-right">
				<div class="play"  :class="payClass" @click="totalmoney">{{payDesc}}</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default{
		props: {
		  "selectFoods":{
		  	type:Array,
		  	default() {
		  		return [];
		  	}
		  },
	      "deliveryPrice": {
		        type: Number,
		        default: 0
		      },
		      "minPrice": {
		        type: Number,
		        default: 0
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
		 	payDesc(){
		        if (this.totalPrice === 0) {
		          return `${this.minPrice}元起送`
		        } else if (this.totalPrice < this.minPrice) {
		          let diff = this.minPrice - this.totalPrice
		          return `还差${diff}元起送`
		        } else {
		          return '去结算'
		        }
		    },
		    payClass(){
		        if (this.totalPrice < this.minPrice) {
		          return 'not-enough'
		        } else {
		          return 'enough'
		        }
		     },
		 	totalCount(){
		        let count = 0
		        this.selectFoods.forEach((food) => {
		          count += food.count
		        })
		        return count
		    }
		 },
		 methods:{
		 	totalmoney(){
		 		if (this.totalPrice < this.minPrice) {
		          alert('还没到起送金额')
		        } else {
		          alert("需要支付" + this.totalPrice + '元')
		        }
		 	}
		 }

	};
</script>

<style>
	.shopcart{position:fixed;left:0;bottom:0;z-index:50;width:100%;height:48px;}
	.shopcart .content{display:flex;background:#141d27;margin-left:0px;color:rgba(255,255,255,0.4);}
	.shopcart .content .content-left{flex:1;}
	.shopcart .content-left .logo-wrapper{display:inline-block;position:relative;top:-10px;margin:0 12px;padding:6px;width:56px;height:56px;box-sizing:border-box;vertical-align:top;border-radius:50%;background:#141d27;}
	.shopcart .content-left .logo{width:100%;height:100%;border-radius:50%;background:#2b343c;text-align:center;}
	.shopcart .content-left .num{position:absolute;top:0;right:0;width:24px;height:16px;line-height:16px;text-align:center;border-radius:16px;font-size:9px;font-weight:700;color:#fff;background:rgb(240, 20, 20);box-shadow:0 4px 8px 0 rgba(0, 0, 0, 0.4);}
	.shopcart .content-left .icon-shopping_cart{line-height:44px;font-size:24px;color:#80858a;}
	.shopcart .logo-wrapper .hightlight{background:#00a0dc;}
	.shopcart .content-left .price{display:inline-block;vertical-align:top;margin-top:12px;line-height:24px;padding-right:12px;box-sizing:border-box;border-right:1px solid rgba(255,255,255,0.1);font-size:16px;font-weight:700;color:rgba(255,255,255,0.4);}
	.shopcart .content-left .hightlight{color:#fff;}
	.shopcart .content-left .desc{display:inline-block;vertical-align:top;margin:12px 0 0 12px;line-height:24px;color:rgba(255,255,255,0.4);font-size:10px;}
	.shopcart .content .content-right{flex:0 0 105px;width:105px;}
	.shopcart .content-right .play{height:48px;line-height:48px;text-align:center;font-size:12px;font-weight:700;background:#2b333b;cursor:pointer;}
	.shopcart .content-right .not-enough{background:#26333b;}
	.shopcart .content-right .enough{background:#00b43c;color:#fff;}
</style>