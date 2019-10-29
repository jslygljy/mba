<template>
    <view class="taskDetail">
		<uni-countdown 
		:showDay="false"
			:day="0" 
			:hour="0" 
			:minute="0" 
			:second="0">
		</uni-countdown>
		<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="false" :interval="2000" :duration="500" current="curryIndex" @change="change">
			<swiper-item v-for="(item, index) in list" :key="index">
				<view class="header">
					<text>{{item.ttitle}}</text>
					<view class="right-progress">
						<text class="text-blue">{{curryIndex+1}}</text>
						/
						<text>{{allIndex}}</text>
					</view>
				</view>
				<view class="content">
					<text>({{item.type==1? '单选题':'多选题'}}){{item.title}}</text>
				</view>
				<view class="ans-list">
					<view class="ans-item" v-for="(subitem, subindex) in item.item_list" :key="subindex" @click="setChoose(index,subindex,subitem)">
						<view class="item-left flex-sub">
							<view :class="['border-info',subitem.isChoose?'border-info2':'']">{{subitem.option}}</view>
						</view>
						<view class="item-right">{{subitem.content}}</view>
					</view>
				</view>
			</swiper-item>
		</swiper>
		<neil-modal
		    :show="isShow" 
		    @close="closeModal" 
		    title="答题卡" 
		    @cancel="bindBtn('cancel')" 
			confirmText="交卷并查看结果"
		    @confirm="bindBtn('confirm')">
			<text class="modal_info">
				多种题型综合
			</text>
			<view class="flex">
				<view class="border-info3 flex-sub">
					1
				</view>
				<view class="border-info3 flex-sub">
					2
				</view>
				<view class="border-info3 flex-sub">
					3
				</view>
				<view class="border-info3 flex-sub">
					4
				</view>
				<view class="border-info3 flex-sub">
					5
				</view>
			</view>
		</neil-modal>
		<view class="" v-if="showdetail">
			getinfo
		</view>
	</view>
</template>

<script>
	import uniCountdown from "@/components/uni-countdown/uni-countdown.vue"
	import config from '../../config.js';
	import neilModal from '@/components/neil-modal/neil-modal.vue';
    export default {
		 components: {uniCountdown,neilModal},
        data() {
			return {
				ttitle:'',
				indicatorDots:false,
				curryIndex:0,
				allIndex:5,
				type:'',
				list:[],
				item_list:[],
				ansList:[],
				isShow:false,
				showdetail:false
			}   
         },
		 onShow(){
		 	this.getDetail();
		 },
		 onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		    this.topicid = option.topicid;
		    this.title = option.title;
			this.subTitle = option.subTitle;
			this.pages =option.pages;
			this.showdetail = option.showdetail ==="false" ? false : true;
		 },
        methods: {
			objectChange(e){
				this.curryIndex = e.tab.value
			},
			getDetail(){
				let id =uni.getStorageSync('customer_id');
				// 获取做题列表
				uni.request({
					url: config.url+'/app/qa/list?special_work='+this.title+'&epoint='+this.subTitle+'&pageindex='+this.pages, //仅为示例，并非真实接口地址。
				    data: {
				    },
				    success: (res) => {
						if(res.data.errcode==0&&res.data.data.length>0){
							res.data.data.map((data)=>{
								data.item_list.map((data2)=>{
									data2.isChoose = false;
								})
							})
							this.list = res.data.data;
						}
						
				    }
				});
			},
			change(e){
				this.curryIndex = e.detail.current
			},
			setChoose(index,subindex,subitem){
				this.list[index].item_list.map((data2)=>{
					data2.isChoose = false;
				})
				
				if(!this.ansList[index]){
					this.ansList.push({
						qa_id:subitem.qa_id,
						is_true:subitem.is_true,
						answer:subitem.option
					});
				}else{
					this.ansList[index].qa_id = subitem.qa_id;
					this.ansList[index].is_true = subitem.is_true;
					this.ansList[index].answer = subitem.option;
				}
				
				this.list[index].item_list[subindex]['isChoose'] = true;
				if(this.ansList.length==5){
					this.isShow = true
				}
				
			},
			bindBtn(type){
				uni.navigateTo({
				    url: '../report/report?id='+this.topicid+'&title='+this.title+'&subTitle='+this.subTitle+'&pages=0&showdetail=false'+'&list='+JSON.stringify(this.ansList)
				});
			},
			closeModal(){
				this.isShow = false
			}
        }
    }
</script>

<style scoped lang="scss">
	uni-page-body{
		height: 100%;
	}
	uni-swiper {
	    height: 100%;
	}
	uni-swiper-item{
		overflow-y: scroll;
		overflow-x: hidden;
	}
    .taskDetail{
		width:100%;
		height: 100%;
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
</style>
