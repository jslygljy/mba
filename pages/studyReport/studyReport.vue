<template>
	<view>
		<text>学习报告时间更新至2019-12-07 11:07:29</text>
		<view class="days">
			【MBA大师】陪我度过<text>1</text>天
			<uni-icon type="help"></uni-icon>
		</view>
		<view class="flex solid-bottom">
			<view class="item">
				<text>
					<text class="text-xl">0</text>
					天</text>
				<text>连续学习</text>
			</view>
			<view class="item">
				<text>
					<text class="text-xl">0</text>
					小时</text>
				<text>视频学习</text>
			</view>
			<view class="item">
				<text>
					<text class="text-xl">0</text>
					个</text>
				<text>做题总数</text>
			</view>
		</view>
		<view class="">
			<view class="">
				
			</view>
			<view class="">
				
			</view>
		</view>
		<view class="">
			
		</view>
	</view>
</template>

<script>
	import clTabs from '@/components/cl-tabs/cl-tabs.vue'
	export default {
		components:{
			clTabs
		},
		data() {
			return {
				tabBars:[
					{tab:'关注',scroll:0},
					{tab:'关注',scroll:0}
				],
				tabCurrentIndex:-1
			}
		},
		mounted(){
			this.tabCurrentIndex = 1
		},
		methods: {
			tabChange(e){
				
				this.tabCurrentIndex = e
				
			}
		}
	}
</script>

<style scoped lang="scss">
.top-box{
	position: absolute;
	z-index: 10;
}
.top{
	height: 40px;
	display: flex;
	flex-direction: row;
	justify-content: center;
	align-items: center;
	margin: 0 20rpx;
	width: 710rpx;
}

.search{
	flex: 1;
	height: 30px;
	background-color: #f5f5f5;
	border-radius: 20px;
	padding: 0 20rpx;
	font-size: 14px;
	display: flex;
	align-items: center;
}

.swiper{
	position: relative;
	
}

.scroll-mask{
	width: 100%;
	height: 100%;
	overflow: hidden;
}

.scroll-Y{
	width: 100%;
	height: 100%;
}
.scroll-box{
	margin: 20rpx;
	height: 40px;
}
</style>
