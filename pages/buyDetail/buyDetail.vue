<template>
    <view class="buy-detail">
		<image :src="content" mode="widthFix" style="width: 100%;height: 100%;position: fixed;padding-bottom: 100rpx;"></image>
		<view class="comment-post">
			<view class="">
				<text>{{price}}</text>
				<text>{{old_price}}</text>
				<text>{{buy_count}}人已购</text>
			</view>
			<text class="post-button2" @click="reportInfo">立即购买</text>
		</view>
    </view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import comment from '@/components/commoent/uni-comment.vue';
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item-curlum/uni-collapse-item-curlum.vue'
	import config from '../../config.js';
    export default {
		components:{
			sunTab,
			comment,
			uniCollapse,
			uniCollapseItem
		},
        data() {
            return {
				content:'',
				price:'',
				buy_count:''
            }
        },
		onShow(){
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		   this.price = option.price;
		   this.old_price = option.old_price;
		   this.content = option.content;
		   this.buy_count = option.buy_count;
		   this.course_id = option.innerid;
		   uni.setNavigationBarTitle({ title: option.title })
		},
        methods: {
			reportInfo(){
				let id = uni.getStorageSync('customer_id');
				
				// 推荐课程
				uni.request({
					url: config.url + '/app/wechat/unifiedOrder/get', //仅为示例，并非真实接口地址。
					method:"POST",
					data: {
						course_id: this.course_id,
						customer_id:id
					},
					success: (res) => {
						let obj = {
							appid: res.data.data.appid,
							noncestr: res.data.data.nonce_str,
							package: 'Sign=WXPay',
							partnerid:res.data.data.partnerid,
							prepayid:res.data.data.prepay_id,
							timestamp:res.data.data.timestamp,
							sign:res.data.data.sign
						}
						// 第一种写法，传对象
						let orderInfo = JSON.stringify(obj)
						uni.requestPayment({
						    provider: 'wxpay',
						    orderInfo: orderInfo, //微信、支付宝订单数据
						    success: function (res) {
						        console.log('success:' + JSON.stringify(res));
						    },
						    fail: function (err) {
						        console.log('fail:' + JSON.stringify(err));
						    }
						})
					}
				});
			}
        }
    }
</script>

<style scoped lang="scss">
.buy-detail{
	background-color: #fff;
	width: 100%;
	height: 100%;
	
	.comment-post{
		position: fixed;
		bottom: 0px;
		height: 100rpx;
		display: flex;
		width: 100%;
		background: #fff;
		z-index: 1;
		input{
			flex:3;
			margin: 20rpx 0rpx 0px 30rpx;
			background-color: #eee;
			text-indent: 20rpx;
			font-size: 30rpx;
			height: 60rpx;
			color: #eef;
			border-radius: 10rpx;
		}
		.post-button{
			flex:1;
			margin: 20rpx 30rpx 0px 30rpx;
			border-radius: 40rpx;
			font-size: 28rpx;
			text-align: center;
			color: #fff;
			height: 60rpx;
			line-height: 60rpx;
			background-color: blue;
		}
		.post-button2{
			flex:1;
			margin: 20rpx 100rpx 0px 100rpx;
			border-radius: 40rpx;
			font-size: 28rpx;
			text-align: center;
			color: #fff;
			height: 70rpx;
			line-height: 70rpx;
			background-color: blue;
		}
	}
	
	
	
}
</style>
