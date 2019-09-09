<template>
    <view class="readDetail">
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" bgColor="#fff" rangeKey="name" :scroll="false" style="width:250rpx;margin-left:250rpx;"></sun-tab>
		<view class="page-main" v-if="curryIndex==1">
			<text class="main-title">
				附近饭店烧烤了房间看了电视剧看了房间里看见了空间来看
			</text>
			<image src="https://iconfont.alicdn.com/t/1498808962006.png@200h_200w.jpg" mode="" class="main-logo"></image>
			<image src="https://iconfont.alicdn.com/t/1498808962006.png@200h_200w.jpg" mode="" class="main-matching"></image>
			<text class="main-font">
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
			</text>
		</view>
		<view class="page-info" v-if="curryIndex==2">
			<text class="main-title">
				附近饭店烧烤了房间看了电视剧看了房间里看见了空间来看2
			</text>
			<image src="https://iconfont.alicdn.com/t/1498808962006.png@200h_200w.jpg" mode="" class="main-logo"></image>
			<image src="https://iconfont.alicdn.com/t/1498808962006.png@200h_200w.jpg" mode="" class="main-matching"></image>
			<text class="main-font">
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
				富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧富家大室考虑考虑剧看了极乐空间从V领抗锯齿昆仑雪菊快乐才相聚离开从小就离开不见了恐惧
			</text>
		</view>
		<button class="cu-btn round lg bg-grey bg-bule">完成学习</button>
    </view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	
    export default {
		components:{
			sunTab
		},
        data() {
			return {
			   index: 0,
				   tabList: ['原文','详解'], //普通数据赋值
				   tabObjectList: [ //对象数组赋值
					   {
						   name: '原文',
						   value: 1
					   },
					   {
						   name: '详解',
						   value: 2
					   }
				   ],
				   curryIndex:0
			   }
         },
        methods: {
			objectChange(e){
				this.curryIndex = e.tab.value
			    console.log();
			},
        }
    }
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
    .readDetail{
		width:100%;
		.uni-tab-item{
			margin-left: 30rpx;
		}
		.page-main,.page-info{
			padding: 40rpx;
		}
		.main-title{
			font-size: 38rpx;
			font-weight: bold;
			line-height: 50rpx;
			display: block;
		}
		.main-logo{
			width: 100rpx;
			height: 30rpx;
			margin: 20rpx 0px;
		}
		.main-matching{
			width:100%;
			height: 200rpx;
		}
		.main-font{
			margin-top: 30rpx;
			line-height: 22rpx;
			font-size: 24rpx;
		}
		.cu-btn{
			width: 300rpx;
			left: 230rpx;
			bottom:30rpx;
			position: fixed;
		}
		.bg-bule{
			background-color: #0081ff;
		}
	}
	
</style>
