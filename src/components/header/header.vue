<template>
    <div class="header">
        <div class="content-wrapper">
        	<div class="avatar">
        		<img width='64' height='64' :src="seller.avatar">
        	</div>
        	<div class="content">
        		<div class="title">
        			<span class="brand"></span>
        			<span class="sellername">{{seller.name}}</span>
        		</div>
        	
	        	<div class="description">
	        		{{seller.description}}/{{seller.deliveryTime}}分钟送达
	        	</div>

	        	<div v-if="seller.supports" class="support">
	        		<span class="icon"></span>
	        		<span class="text">{{seller.supports[0].description}}</span>
	        	</div>
            </div>
            <div v-if='seller.supports' class="support-count" @click="showDetail">
                <span class="count">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-wrapper" @click="showDetail">
        	<span class="bulletin-title"></span>
            <span class="bulletin-text">{{seller.bulletin}}</span>
             <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="background">
            <img :src="seller.avatar" width='100%' height="100%">
        </div>
        <div v-show="detailShow" class="detail">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                   <h1 class="name">{{seller.name}}</h1>
                </div>
            </div>
            <div class="detail-close" @click="closeDetail">
                <i class="icon-close"></i>
            </div>
        </div>
    </div>
</template>

<script>
    export default{
    	props:{
    		seller:{
    			type:Object
    		}
    	},
        data(){
            return{
                detailShow:false
            };
        },
        methods:{
            showDetail() {
                this.detailShow = true;
            },
            closeDetail() {
                this.detailShow = false;
            }
        }
    }
</script>

<style>
	.header{position:relative;color:#fff;background:rgba(7,17,27,0.5);overflow:hidden;}
	.content-wrapper{position:relative;padding:24px 12px 18px 24px;font-size:0}
	.avatar{display:inline-block}
    .avatar img{border-radius:2px;}
	.content{display:inline-block;margin-left:16px;font-size:14px;}
	.title{margin:2px 0 8px 0;}
	.brand{display:inline-block;vertical-align:top;width:30px;height:18px;background:url('../../../img/brand.png') no-repeat center;background-size:30px 18px}
	.sellername{display:inline-block;vertical-align:top;margin-left:6px;font-size:16px;line-height:18px;font-weight:bold;}
	.description{margin-bottom:10px;line-height:12px;font-size:12px;}
    .support .icon{display:inline-block;width:12px;height:12px;margin-right:4px;background:url('../../../img/decrease.png') no-repeat center;background-size:12px 12px;}
    .support .text{line-height:12px;font-size:10px;}
    .support-count{position:absolute;right:12px;bottom:14px;padding:0 8px;height:24px;line-height:24px;border-radius:14px;background:rgba(0,0,0,0.2);text-align:center;}
    .count{font-size:10px;vertical-align:top;}
    .icon-keyboard_arrow_right{margin-left:2px;line-height:24px;font-size:10px;}
    .bulletin-wrapper{position:relative;height:28px;line-height:28px;padding:0 22px 0 12px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
    .bulletin-wrapper .bulletin-title{display:inline-block;vertical-align:top;margin-top:9px;width:22px;height:12px;background:url('../../../img/bulletin.png') no-repeat center;background-size:22px 12px;}
   .bulletin-wrapper .bulletin-text{vertical-align:top;margin:0 4px;font-size:10px;}
   .bulletin-wrapper .icon-keyboard_arrow_right{position:absolute;font-size:10px;right:12px;top:5px;}
   .background{position:absolute;top:0;left:0;width:100%;height:100%;z-index:-1;filter:blur(10px)}
   .header .detail{position:fixed;z-index:100;top:0;left:0;width:100%;height:100%;overflow:hidden;background:rgba(7,17,27,0.5);}
   .detail-wrapper{min-height:100%;width:100%;}
   .detail-main{margin-top:64px;padding-bottom:64px;}
   .detail-main .name{line-height:16px;text-align:center;font-size:16px;font-weight:700;}
   .detail-close{position:relative;width:32px;height:32px;margin:-64px auto 0 auto;clear:both;font-size:32px;}
</style>