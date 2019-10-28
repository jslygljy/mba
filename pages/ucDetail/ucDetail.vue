<template>
	<view class="ucDetail">
		<!--   头部  -->
		<view class="bg-blue" style="padding:30rpx 30rpx;">
			<view class="flex lists">
				<view class="flex-sub">
					<image :src="form.logo" mode="" style="width: 150rpx;height: 150rpx;margin: 0px 24rpx;"></image>
				</view>
				<view class="flex-treble">
					<view class="uni-list-item__container">
					  <view class="uni-list-item__content">
						  <view class="title2">{{ form.name }}</view>
					  </view>
					  <view class="uni-list-item__container">
					  	<view class="title">创办时间: {{form.establish_time}}</view>				  
					  </view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- form   -->
		<view class="formInfo">
			<text class="info-title">基本信息</text>
			<view class="list">
				<text class="left">地区:</text>
				<text class="right">{{form.area}}</text>
			</view>
			<view class="list">
				<text class="left">性质:</text>
				<text class="right">{{form.nature}}</text>
			</view>
			<view class="list">
				<text class="left">地址:</text>
				<text class="right">{{form.address}}</text>
			</view>
			<text class="info-title">招生信息</text>
			<view class="list">
				<text class="left">项目类别:</text>
				<text class="right">{{form.item_category}}</text>
			</view>
			<view class="list">
				<text class="left">学制:</text>
				<text class="right">{{form.edu_system}}</text>
			</view>
			<view class="list">
				<text class="left">是否接受调剂:</text>
				<text class="right">{{form.receive_adjust==1?'是':'否'}}</text>
			</view>
			<view class="list">
				<text class="left">是否开始提前面试:</text>
				<text class="right">{{form.adv_interview==1?'是':'否'}}</text>
			</view>
			<view class="list">
				<text class="left">招生人数:</text>
				<text class="right">{{form.enrolment_count}}</text>
			</view>
			<view class="list">
				<text class="left">分数线类别:</text>
				<text class="right">{{form.grade_category}}</text>
			</view>
			<view class="list">
				<text class="left">学费:</text>
				<text class="right">{{form.tuition}}</text>
			</view>
			<view class="list">
				<text class="left">历年分数线:</text>
				<text class="right">{{form.his_grade_line}}</text>
			</view>
			<text class="info-title">院校官网</text>
			<view class="list">
				<text class="list-link"><a :href="form.website">{{form.website}}</a></text>
			</view>
		</view>
	</view>
</template>

<script>
	import config from '../../config.js';
	export default {
		data() {
			return {
				innerid:'',
				form:{},
				webviewStyles: {
					progress: {
						color: '#FF3333'
					}
				}
			};
		},
		onLoad: function (option) {
		   this.innerid = option.innerid;
		},
		onShow(){
			this.getDetail();
		},
		methods: {
			getDetail(){
				// 获取做题列表
				uni.request({
					url: config.url+'/app/uc/detail/'+this.innerid,
				    data: {
				    },
				    success: (res) => {
						this.form=res.data.data[0]
				    }
				});
			},
			gotoList(){
				plus.runtime.openURL('https://baidu.com')
			}
		}
	}
</script>

<style lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
.ucDetail{
	.title2{
		font-size:28rpx;
	}
	.title{
		font-size: 24rpx;
		margin-top: 70rpx;
	}
	.formInfo{
		.info-title{
			margin: 20rpx 20rpx;
			font-size: 30rpx;
			font-weight: bold;
			display: inline-block;
		}
		.list{
			display: flex;
			font-size: 26rpx;
			margin: 20rpx;
			.left{
				flex:1;
				text-align: right;
			}
			.right{
				flex:2;
				text-align: left;
				margin-left: 10rpx;
			}
			.list-link{
				margin-left: 50rpx;
				a{
					text-decoration: none;
					color: #0081FF;
				}
			}
		}
	}
}
</style>
