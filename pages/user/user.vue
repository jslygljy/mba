<template>
    <view class="my-content">
		<view class="a-swiper-dot">
			<view class="cu-list menu-avatar">
				<view class="cu-item">
					<view class="cu-avatar round lg" style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);"></view>
					<view class="content flex-sub">
						<view class="text-grey">
							U52832832 
							<text class="cuIcon-crownfill margin-left-sm"></text>
						</view>
						<view class="text-gray text-sm flex cu-capsule">
							<text class="cuIcon-moneybag text-xl text-gold "></text>
							<text class="margin-left-xs">0</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="padding">
			<view class="bg-gradual-blue padding radius shadow-blur radius-round">
				<view class="flex justify-between">
					<view class="text-lg">我的课程</view>
					<view class="text-sm">查看全部 ></view>
				</view>
				<view class="flex justify-start margin-top-sm">
					<button class="cu-btn line-white text-sm margin-right-sm margin-top-sm">
						<text class="cuIcon-playfill"></text>
						<text style="margin-left: 5upx;">继续观看</text>
					</button>
					<view class="margin-top text-Abc text-sm">助力20备考</view>
				</view>
			</view>
		</view>
        <view class="card-menu margin-top cu-list menu">
			<view class="cu-item arrow">
				<view class="content">
					<text class="cuIcon-down text-blue"></text>
					<text class="text-grey">下载管理</text>
				</view>
			</view>
			<view class="cu-item arrow">
				<view class="content">
					<text class="cuIcon-writefill text-blue"></text>
					<text class="text-grey">我的错题</text>
				</view>
			</view>
			<view class="cu-item arrow">
				<view class="content">
					<text class="cuIcon-favorfill text-blue"></text>
					<text class="text-grey">我的收藏</text>
				</view>
			</view>
		</view>
		<view class="borderInfo"></view>
		<view class="card-menu margin-top cu-list menu">
			<view class="cu-item arrow" @click="toFriend">
				<view class="content">
					<text class="cuIcon-profile text-yellow"></text>
					<text class="text-grey">推荐好友</text>
				</view>
				<view class="action" style="margin-top: -10rpx;">
					<text class="text-yellow text-sm">邀请可得40元学习大礼包</text>
				</view>
			</view>
			<view class="cu-item arrow">
				<view class="content">
					<text class="cuIcon-present text-yellow"></text>
					<text class="text-grey">我的优惠券</text>
				</view>
			</view>
			<view class="cu-item arrow">
				<view class="content">
					<text class="cuIcon-form text-yellow"></text>
					<text class="text-grey">订单记录</text>
				</view>
			</view>
		</view>
		<view class="borderInfo"></view>
		<view class="card-menu margin-top cu-list menu">
			<view class="cu-item arrow">
				<view class="content">
					<text class="cuIcon-settingsfill text-gray"></text>
					<text class="text-grey">设置</text>
				</view>
			</view>
			<view class="cu-item arrow">
				<view class="content">
					<text class="cuIcon-questionfill text-gray"></text>
					<text class="text-grey">帮助中心</text>
				</view>
			</view>
		</view>
    </view>
</template>

<script>
	
    export default {
		components:{
		},
        
        methods: {
            toFriend(){
				uni.reLaunch({
				    url: '../inviteFriend/inviteFriend',
				});
			}
        }
    }
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
	.my-content{
		width: 100%;
	}
	.borderInfo{
		border-top:1rpx #ccc solid;
		margin-right: 40upx;
		margin-left: 20upx;
		margin-top: 20px;
		margin-bottom: 20px;
		height: 2upx;
	}
	.cu-btn{
		padding:0px 15upx;
		height: 52rpx;
	}
	.cu-btn[class*="line"]::after{
		border-radius: 200upx;
	}
</style>
