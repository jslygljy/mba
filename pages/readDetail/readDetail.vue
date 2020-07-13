<template>
    <view class="readDetail">
		<!-- <sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" bgColor="#fff" rangeKey="name" :scroll="false" style="width:250rpx;margin-left:250rpx;"></sun-tab> -->
		<view class="page-main">
			<text class="main-title">
				{{item.title}}
			</text>
			<image :src="item.cover" mode="" class="main-matching"></image>
			<view class="main-font">
				<u-parse :content="item.content" />
			</view>
		</view>
		<!-- <button class="cu-btn round lg bg-grey bg-bule">完成学习</button> -->
    </view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import neilModal from '@/components/neil-modal/neil-modal.vue';
	import config from '../../config.js';
	import uParse from '@/components/gaoyia-parse/parse.vue'
    export default {
		components:{
			sunTab,
			neilModal,
			uParse
		},
        data() {
			return {
			   item:{},
			   id:''
			}
         },
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		   this.id = option.id;
		},
		onShow(){
		  this.getErrDetail()
		  
		},
        methods: {
			getErrDetail(){
				let id =uni.getStorageSync('customer_id');
				// 获取做题列表
				uni.request({
					url: config.url+'/app/read/detail/'+this.id ,
				    data: {
				    },
				    success: (res) => {
						this.item = res.data.data[0];
				    }
				});
			},
        }
    }
</script>

<style scoped lang="scss">
    .readDetail{
		height: 100%;
		display: flex;
		flex-direction: column;
		background-color: #fff;
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
			margin-top: 20rpx;
		}
		.main-font{
			margin-top: 30rpx;
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
