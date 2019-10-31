<template>
	<view class="history-content">
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name"></sun-tab>
		<!-- <view class="paper-classify-item-name"><i class="right-point"></i><text>联考真题题库</text><i class="left-point"></i></view> -->
		<uni-list class="list-info">
			<view class="flex solids-bottom lists" @click="onClick" v-for="(item, index) in list" :key="item.topicid">
				<view class="flex-four">
					<view class="uni-list-item__container">
					  <view class="uni-list-item__content">
						  <view class="uni-list-item__content-title2">{{ item.title }}</view>
					  </view>
					</view>
					<view class="uni-list-item__container">
						<view class="uni-list-item__content-title">共{{item.num}}道题,答对{{item.is_true}}道</view>				  
					</view>
				</view>
				<view class="flex-sub">
					<text class="right-time">{{item.time}}</text>
				</view>
			</view>
		</uni-list>
		<view class="text-center" v-if="list.length==0">
			<text>暂无数据</text>
		</view>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import config from '../../config.js';
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	export default {
		components: {
			uniList,
			sunTab
		},
		data() {
			return {
				title:'123',
				index:0,
				list:[],
				tabObjectList: [ //对象数组赋值
					{
						name: '专项练习',
						value: 0,
					},
					{
						name: '套题练习',
						value: 1,
					}
				]
			}
		},
		onLoad() {

		},
		onShow() {
			this.getList();

		},
		methods: {
			objectChange(e){
				this.index=e.tab.value;
				this.getList();
			},
			getList() {
				let id = uni.getStorageSync('customer_id');
				// 推荐课程
				if(this.index==0){
					uni.request({
						url: config.url + '/app/qa/history/list/'+id, //仅为示例，并非真实接口地址。
						method:"GET",
						data: {
						},
						success: (res) => {
							this.list = res.data.data
						}
					});
				}else{
					uni.request({
						url: config.url + '/app/qa/history/topic/'+id, //仅为示例，并非真实接口地址。
						method:"GET",
						data: {
						},
						success: (res) => {
							this.list = res.data.data;
						}
					});
				}
			},
			onClick(){
				
			}
			
		}

	}
</script>

<style lang="scss">

.history-content{
	width: 100%;
	background-color: #fff;
}
@mixin list-disabled {
	opacity: 0.3;
}
.paper-classify-item-name{
	background: rgba(32,102,255,.1);
	border-radius: 8rpx;
	text-align: center;
	margin-bottom: 30rpx;
	margin-top: 60rpx;
}
.list-info{
	margin-top: 20rpx;
}
.right-time{
	margin-top: 80rpx;
	vertical-align: top;
	display: inline-block;
	color: #666;
	font-size: 24rpx;
}
.right-point {
    width: 50rpx;
    height: 14rpx;
	display: inline-block;
    background: url(../../static/right2.png) no-repeat;
    background-size: 100% 100%;
    vertical-align: middle;
}
.left-point {
    width: 50rpx;
    height: 14rpx;
	display: inline-block;
    background: url(../../static/left.png) no-repeat;
    background-size: 100% 100%;
    vertical-align: middle;
}
.paper-classify-item-name text {
    vertical-align: middle;
    font-size: 34rpx;
    font-family: PingFang-SC-Bold;
    font-weight: 700;
    color: #2066ff;
    padding: 20rpx 10rpx;
}
$list-item-pd: $uni-spacing-col-lg $uni-spacing-row-lg;
.progress{
	width: 70%;
	margin-left: 60rpx;
}
.lists{
}
.blueRound{
	display: inline-block;
	width: 15rpx;
	height: 15rpx;
	background-color: dodgerblue;
	vertical-align: top;
	border-radius: 50%;
	margin-top: 14rpx;
	margin-right: 20rpx;
}
.edit-button{
	position: absolute;
	right: 60rpx;
    top: 20rpx;
	font-size: 50rpx;
}
.flex-four{
	padding-bottom: 10rpx;
}
.uni-list-item {
	font-size: $uni-font-size-lg;
	position: relative;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	margin-top: 10rpx;
	height: 150rpx;
	&--disabled {
		@include list-disabled;
	}

	&__container {
		padding: 14rpx 24rpx 6rpx;
		width: 100%;
		box-sizing: border-box;
		flex: 1;
		position: relative;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}

	&__content {
		flex: 1;
		display: flex;
		color: #3b4144;
		&-title {
			font-size: $uni-font-size-lg;
			text-overflow: ellipsis;
			white-space: nowrap;
			color: inherit;
			line-height: 1.5;
			font-size: 24rpx;
			overflow: hidden;
			display: inline-block;
			color: #666666;
		}
		&-title2 {
			font-size: $uni-font-size-lg;
			text-overflow: ellipsis;
			white-space: nowrap;
			color: inherit;
			font-size: 32rpx;
			line-height: 1.5;
			overflow: hidden;
			display: inline-block;
		}

		&-note {
			margin-top: 6upx;
			color: $uni-text-color-grey;
			font-size: $uni-font-size-base;
			white-space: normal;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
		}
	}

	&__extra {
		width: 25%;
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
		align-items: center;
	}

	&__icon {
		margin-right: 18upx;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;

		&-img {
			height: $uni-img-size-base;
			width: $uni-img-size-base;
		}
	}
}

.uni-list > .uni-list-item:last-child .uni-list-item-container:after {
	height: 0px;
}
</style>

