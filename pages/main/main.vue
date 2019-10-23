<template>
	<view class="content">
		<bw-swiper :swiperList="swiperList" style="width:100%"></bw-swiper>
		<view class="list">
			<view class="item">
				<text class="icon1 i"></text>
				<text>书城</text>
			</view>
			<view class="item" @click="goToRead">
				<text class="icon2 i"></text>
				<text>每日阅读</text>
			</view>
			<view class="item" @click="goToCurriculum">
				<text class="icon3 i"></text>
				<text>我的课程</text>
			</view>
			<view class="item">
				<text class="icon4 i"></text>
				<text>学习报告</text>
			</view>
		</view>
		<!-- <view class="ad">
			<image src="../../static/main/activity1.jpg" mode=""></image>
		</view> -->
		<view>
			<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name" :scroll="true" style="width: 95%;float: left; "></sun-tab>
			<view style="height: 80upx;line-height: 90upx;"><image src="../../static/right.png" style="width: 32upx;height: 32upx;line-height: 80upx;"></image></view>
		</view>

		<!--  推荐课程  -->
		<view v-if="index==0">
			<h3 class="list-title">推荐课程</h3>
			<view class="list-item" v-for="(item,index) in tuijianlist" :key="item.innerid">
				<view class="list-item-content" @click="goToDetail(item.innerid,item.is_sgin)">
					<image :src="item.speaker_heading" mode=""></image>
					<view class="item-right">
						<view style="flex:4">
							<h4 class="h4">{{item.title}}</h4>
							<text class="grey">{{item.subinfo}}</text>
						</view>
						<view class="item-bottom">
							<text class="price">{{item.price}}</text>
							<text class="sale">{{item.old_price}}</text>
							<text class="buy">{{item.buy_count}}人已购</text>
						</view>
					</view>
				</view>
			</view>
		</view>


		<!--  免费系统课程  -->
		<view style="margin-bottom: 40upx;">
			<h3 class="list-title">{{subName}}</h3>
			<view class="list-item" v-for="(item,index) in mianfeilist" :key="item.innerid">
				<view class="list-item-content" @click="goToDetail(item.innerid,item.is_sgin)">
					<image :src="item.speaker_heading" mode="" class="people"></image>
					<view class="item-right">
						<view style="flex:5">
							<h4 class="h4">{{item.title}}</h4>
							<text class="grey">{{item.subinfo}}</text>
						</view>
						<view class="item-bottom">
							<text class="price">免费</text>
							<text class="buy">{{item.buy_count}}人已报名</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import bwSwiper from '@/wxcomponents/bw-swiper/bw-swiper.vue'
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import config from '../../config.js';
	export default {
		components: {
			bwSwiper,
			sunTab
		},
		data() {
			return {
				hasLogin: false,
				swiperList: [{
					img: '../../static/main/banner1.png'
				}, {
					img: '../../static/main/banner2.png'
				}, {
					img: '../../static/main/banner3.png'
				}],
				index: 0,
				mianfeilist: [],
				tuijianlist: [],
				subName: '免费推荐',
				tabObjectList: [ //对象数组赋值
					{
						name: '精品推荐',
						value: 0,
						id: 0,
						subName: '免费推荐'
					},
					{
						name: '真题课程',
						value: 1,
						id: 7,
						subName: '真题课程'
					},
					{
						name: '逻辑课程',
						value: 2,
						id: 1,
						subName: '逻辑课程'
					},
					{
						name: '数学课程',
						value: 3,
						id: 2,
						subName: '数学课程'
					},
					{
						name: '英语课程',
						value: 4,
						id: 3,
						subName: '英语课程'
					},
					{
						name: '写作课程',
						value: 5,
						id: 4,
						subName: '写作课程'
					},
					{
						name: '提面及复试',
						value: 6,
						id: 5,
						subName: '提面及复试'
					},
					{
						name: '新手必看',
						value: 7,
						id: 8,
						subName: '新手必看'
					}
				],
			}
		},
		onLoad() {

		},
		onShow() {
			if (this.index == 0) {
				this.getList();
			}

		},
		methods: {
			goToDetail(id, is_sgin) {
				uni.navigateTo({
					url: '../curlumDetail/curlumDetail?course_id=' + id + '&is_sgin=' + is_sgin
				});
			},
			objectChange(e) {
				if (e.tab.value == 0) {
					this.getList();
				} else {
					this.changList(e.tab.id, e.tab.curreylist)
				}
				this.subName = e.tab.subName;
				this.index = e.tab.value;
			},
			goToRead() {
				uni.navigateTo({
					url: '../read/read',
				});
			},
			goToCurriculum() {
				uni.navigateTo({
					url: '../currlum/currlum',
				});
			},
			getList() {
				let id = uni.getStorageSync('customer_id');

				// 推荐课程
				uni.request({
					url: config.url + '/app/course/list/' + id, //仅为示例，并非真实接口地址。
					data: {
						type: 30 
					},
					success: (res) => {
						this.tuijianlist = res.data.data;
					}
				});
				// 免费
				uni.request({
					url: config.url + '/app/course/list/' + id, //仅为示例，并非真实接口地址。
					data: {
						type: 20
					},
					success: (res) => {
						this.mianfeilist = res.data.data;
					}
				});
			},
			changList(type) {
				let userid = uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url + '/app/course/list/' + userid, //仅为示例，并非真实接口地址。
					data: {
						type: type
					},
					success: (res) => {
						if (res.data.errcode == 0) {
							this.mianfeilist = res.data.data
						} else {
							uni.showToast({
								title: res.data.errmsg
							})
						}

					}
				});
			}
		}

	}
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";

	.content {
		padding: 0upx;
	}

	.list-title {
		margin-top: 50rpx;
		margin-left: 15rpx;
	}

	.list-item {
		margin-top: 50rpx;
		margin-left: 15rpx;
		font-size: 40rpx;

		.list-item-content {
			display: flex;
			margin-top: 30rpx;

			image {
				width: 280rpx;
				height: 190rpx;
				min-width: 280rpx;
			}

			.people {
				width: 190rpx;
				height: 190rpx;
				min-width: 190rpx;
			}

			.item-right {
				font-size: 24rpx;
				margin-left: 40rpx;
				display: flex;
				flex-direction: column;

				.h4 {
					font-size: 30rpx;
					margin-top: 2rpx;
				}

				.grey {
					color: #8f8f94
				}

				.item-bottom {
					flex: 1
				}

				.price {
					color: #dc143c;
					font-size: 30rpx;
				}

				.sale {
					margin-left: 20rpx;
					color: #8f8f94;
					text-decoration: line-through;
				}

				.buy {
					color: #8f8f94;
					margin-left: 60rpx;
				}
			}
		}
	}

	.list {
		display: flex;
		margin-bottom: 20rpx;
		margin-top: 20rpx;

		.item {
			flex: 1;
			text-align: center;
			font-size: 24rpx;
		}

		.i {
			margin-left: 60rpx;
			margin-top: 20rpx;
			margin-bottom: 20rpx;
		}
	}

	.ad {

		width: 100%;
		height: 100rpx;
		margin-bottom: 20rpx;

		image {
			margin: 0px 0rpx 0px 10rpx;
			width: 710rpx;
			height: 100%;
		}
	}

	.title {
		color: #8f8f94;
		margin-top: 50upx;
	}

	.icon1 {
		background-image: url('/static/img/bookmark.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}

	.icon2 {
		background-image: url('/static/img/notebook.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}

	.icon3 {
		background-image: url('/static/img/report.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}

	.icon4 {
		background-image: url('/static/img/monitor.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}
</style>
