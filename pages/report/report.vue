<template>
	<view class="box">
		<text>形式逻辑</text>
		<view class="view_box">
			<iCircle
				style="width:300rpx;height:300rpx;margin-left: 100rpx;margin-right: 100rpx;"
				:percent="percent"
				:size="300"
				:stroke-color="color"
				BgId="BgId"
				InId="InId"
			>
				<view class="flex-direction">
					<text class="middeFont">
						<text class="text-bold text-lg">{{success_scode}}
						</text>
					分</text>
					<text class="middeFont text-df margin-top">总分{{total_scode}}分</text>
				</view>
				<view slot="canvas">
					<canvas
						class="CanvasBox strokeCanvas"
						canvas-id="BgId"
					></canvas>
					<canvas
						class="CanvasBox trailCanvas"
						canvas-id="InId"
					></canvas>
				</view>
			</iCircle>
			<view class="flex-sub flex-direction flex">
				<text class="text-bold text-lg">{{success_rate}}%</text>
				<text class="text-df text-grey margin-top">正确率</text>
			</view>
		</view>
		<text class="">
			多钟题型综合
		</text>
		<view class="flex">
			<view :class="['border-info3','flex-sub',data.is_true==0?'bg-red':'bg-green']" v-for="(data,index) in itemList" :key="index">
				{{index+1}}
			</view>
		</view>
		<text class="">
			考点
		</text>
		<view class="button_box">
			<button type="primary" class="button_item" @click="add">+</button>
		</view>
		
		
	</view>
</template>

<script>
	import iCircle from '@/components/xiaoran-circle/xiaoran-circle.vue';
	import config from '../../config.js';
	export default {
		components: {
			iCircle
		},
		data() {
			return {
				percent: 0,
				list:[],
				success_scode:'',
				success_rate:'',
				itemList:[],
				total_scode:''
			}
		},
		onLoad: function (option) { 
		   this.list = JSON.parse(option.list);
		},
		computed: {
			color() {
				let color = '#2db7f5';
				if (this.percent == 100) {
					color = '#5cb85c';
				}
				return color;
			}
		},
		mounted(){
			this.add();
			this.getDetail();
		},
		methods: {
			add() {
				this.percent += 1;
			},
			getDetail(){
				let id =uni.getStorageSync('customer_id');
				// 获取做题列表
				uni.request({
					url: config.url+'/app/qa/customer/edit',
					method:"POST",
				    data: {
						customer_id:id,
						qa_list:this.list
				    },
				    success: (res) => {
						this.itemList = res.data.data.item_list;
						this.success_rate = res.data.data.success_rate;
						this.success_scode = res.data.data.success_scode;
						this.total_scode = res.data.data.total_scode;
				    }
				});
			}
		}
	}
</script>

<style>
	@import "../../static/icon.css";
	@import "../../static/main.css";
	.box {
		/* margin-top: var(--status-bar-height); */
		flex: 1;
		padding: 30rpx;
	}
	.view_box {
		width: 100%;
		height: 100%;
		display: flex;
	}
	.button_box {
		display: flex;
		flex-direction: column;
		align-items: stretch;
		justify-content: flex-start;
		padding: 0 30upx;
	}
	.button_item {
		width: 100%;
		margin-top: 30upx;
	}
	
	.CanvasBox {
		width: 100%;
		height: 100%;
		position: absolute;
		top: 0px;
		left: 0px;
		display: flex;
	}
	.middeFont{
		color: #000;
		font-size: 24rpx;
		display: block;
	}
	.border-info3{
		max-width: 100rpx;
		height: 100rpx;
		font-size: 30rpx;
		line-height: 100rpx;
		border-radius: 50%;
		text-align: center;
		font-weight: bold;
		color: #fff;
		margin: 40rpx 10rpx 50rpx 34rpx;
	}
</style>