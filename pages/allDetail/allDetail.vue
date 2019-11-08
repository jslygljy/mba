<template>
    <view class="errDetail">
		<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="false" :interval="2000" :duration="500" current="curryIndex" @change="change">
			<swiper-item v-for="(item, index) in list" :key="index">
			<view class="header">
				<text>{{item.ttitle}}</text>
				<view class="right-progress">
					<text class="text-blue">{{curryIndex+1}}</text>
					/
					<text>{{qa_id.length}}</text>
				</view>
			</view>
			<view class="content">
				<text>({{item.type==1? '单选题':'多选题'}}){{item.title}}</text>
			</view>
			<view class="ans-list">
				<view class="ans-item" v-for="(subitem, subindex) in item.item_list" :key="subindex">
					<view class="item-left flex-sub">
						<view v-if="subitem.option===item.answer && subitem.is_true ==0" class='border-info border-red'>{{subitem.option}}</view>
						<view v-if="subitem.option!==item.answer && subitem.is_true ==1" class='border-info border-green'>{{subitem.option}}</view>
						<view v-if="subitem.option!==item.answer && subitem.is_true ==0" class='border-info'>{{subitem.option}}</view>
					</view>
					<view class="item-right">{{subitem.content}}</view>
				</view>
			</view>
			</swiper-item>
		</swiper>
		<text class="title">
			本题出自
		</text>
		<text class="info">
			{{item.ttitle}}
		</text>
		<text class="title">
			考点
		</text>
		<text class="info">
			{{item.special_work}}
		</text>
		<text class="title">
			解析
		</text>
		<text class="info">
			{{item.reason}}
		</text>
		
	</view>
</template>

<script>
	import config from '../../config.js';
    export default {
		 components: {},
        data() {
			return {
				ttitle:'',
				indicatorDots:false,
				curryIndex:0,
				allIndex:5,
				type:'',
				items:{},
				item_list:[],
				ansList:[],
				isShow:false,
				showdetail:false,
				qa_id:[],
				answer:''
			}   
         },
		 onShow(){
		 	this.getErrDetail();
		 },
		 onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		    this.qa_id = JSON.parse(option.qa_id);
		 },
        methods: {
			getErrDetail(){
				let id =uni.getStorageSync('customer_id');
				console.log(this.qa_id);
				// 获取做题列表
				uni.request({
					url: config.url+'/app/qa/error/detail/'+this.qa_id[0].qa_id ,
				    data: {
				    },
				    success: (res) => {
						this.items = res.data.data;
						this.list.push(res.data.data);
				    }
				});
			},
			change(e){
				console.log(2);
				this.curryIndex = e.detail.current
			}
        }
    }
</script>

<style scoped lang="scss">
	uni-page-body{
		height: 100%;
		display: flex;
		flex-direction: column;
	}
	uni-swiper {
	    height: 100%;
		display: flex;
		flex-direction: column;
	}
	uni-swiper-item{
		overflow-y: scroll;
		overflow-x: hidden;
	}
	.container{
	  
	}
    .errDetail{
		width:100%;
		padding: 0rpx 20rpx;
		background-color: #fff;
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
				.border-info2{
					color: #fff;
					background-color: #0081ff;
					border:2rpx #0081ff solid;
				}
				.border-green{
					color: #fff;
					background-color: #00C777;
					border:2rpx #00C777 solid;
				}
				.border-red{
					color: #fff;
					background-color: #DD514C;
					border:2rpx #DD514C solid;
				}
			}
			.item-right{
				font-size: 30rpx;
				flex: 5;
				line-height: 46rpx;
				align-self: center;
			}
		}
	}
	.border-info3{
		max-width: 80rpx;
		height: 80rpx;
		border: 2rpx #0081ff solid;
		font-size: 30rpx;
		line-height: 80rpx;
		border-radius: 50%;
		text-align: center;
		font-weight: bold;
		color: #fff;
		background-color: #0081ff;
		margin: 40rpx 10rpx 50rpx 20rpx;
	}
	.modal_info{
		margin: 10rpx 10rpx 20rpx 20rpx;
		display: inline-block;
	}
	.title{
		font-weight: bold;
		font-size: 34rpx;
		padding: 10rpx;
		display: block;
		margin-top: 20rpx;
	}
	.info{
		font-size: 28rpx;
		padding: 10rpx;
		display: block;
	}
</style>
