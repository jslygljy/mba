<template>
    <view class="taskDetail">
		<uni-countdown 
		:showDay="false"
			:day="0" 
			:hour="0" 
			:minute="0" 
			:second="0">
		</uni-countdown>
		<view class="header">
			<text>{{title}}</text>
			<view class="right-progress">
				<text class="text-blue">{{curryIndex}}</text>
				/
				<text>{{allIndex}}</text>
			</view>
		</view>
		<view class="content">
			<text>(单项选择器)福建省大立科技分类考试大减肥快乐圣诞节里看风景昂克赛拉的减肥快乐大数据库了房间卡拉圣诞节疯狂了的萨芬的时刻来访接待室</text>
		</view>
		<view class="ans-list">
			<view class="ans-item" v-for="(item, index) in anslist" :key="index" >
				<view class="item-left flex-sub">
					<view class="border-info">{{item.ides}}</view>
				</view>
				<view class="item-right">{{item.contentInfo}}</view>
			</view>
		</view>
    </view>
</template>

<script>
	import uniCountdown from "@/components/uni-countdown/uni-countdown.vue"
    export default {
		 components: {uniCountdown},
        data() {
			return {
				title:'2013年联考10月真题',
				curryIndex:'1',
				allIndex:'5',
				anslist:[{
					'ides':'A',
					'contentInfo':'123'
				},{
					'ides':'B',
					'contentInfo':'一个范德萨六块腹肌独立思考今飞凯达开了房电视剧里看风景代理商会计分录看电视剧开了房电视剧里看风景的时刻垃圾分类大开杀戒了看电视剧了一个范德萨六块腹肌独立思考今飞凯达开了房电视剧里看风景代理商会计分录看电视剧开了房电视剧里看风景的时刻垃圾分类大开杀戒了看电视剧了'
				},{
					'ides':'C',
					'contentInfo':'一个范德萨六块腹肌独立思考今飞凯达开了房电视剧里看风景代理商会计分录看电视剧开了房电视剧里看风景的时刻垃圾分类大开杀戒了看电视剧了一个范德萨六块腹肌独立思考今飞凯达开了房电视剧里看风景代理商会计分录看电视剧开了房电视剧里看风景的时刻垃圾分类大开杀戒了看电视剧了'
				},{
					'ides':'D',
					'contentInfo':'一个范德萨六块腹肌独立思考今飞凯达开了房电视剧里看风景代理商会计分录看电视剧开了房电视剧里看风景的时刻垃圾分类大开杀戒了看电视剧了一个范德萨六块腹肌独立思考今飞凯达开了房电视剧里看风景代理商会计分录看电视剧开了房电视剧里看风景的时刻垃圾分类大开杀戒了看电视剧了'
				}]
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
    .taskDetail{
		width:100%;
		padding: 0rpx 20rpx;
		.header{
			display: flex;
			justify-content: space-between;
			font-size: 26rpx;
			height: 80rpx;
			line-height: 80rpx;
			border-bottom: 2rpx #ddd solid;
			margin-bottom: 40rpx;
		}
		.content{
			font-size: 32rpx;
			margin-bottom: 60rpx;
		}
		.ans-item{
			display: flex;
			margin-bottom: 80rpx;
			.item-left{
				.border-info{
					width: 80rpx;
					height: 80rpx;
					border: 2rpx #0081ff solid;
					font-size: 30rpx;
					line-height: 80rpx;
					border-radius: 50%;
					text-align: center;
					font-weight: bold;
					color: #0081ff;
					background-color: #fff;
				}
			}
			.item-right{
				font-size: 30rpx;
				flex: 5;
				line-height: 46rpx;
				align-self: center;
			}
		}
		.ans-item:hover .border-info{
			color: #fff;
			background-color: #0081ff;
		}
	}
	
</style>
