<template>
	<view class="box">
		<text class="info-title">
			{{title}}
		</text>
		<view class="view_box">
			<iCircle style="width:300rpx;height:300rpx;margin-left: 100rpx;margin-right: 100rpx;" :percent="percent" :size="300"
			 :stroke-color="color" BgId="BgId" InId="InId">
				<view class="flex-direction">
					<text class="middeFont">
						<text class="text-bold text-lg">{{success_scode || 0}}
						</text>
						分</text>
					<text class="middeFont text-df margin-top">总分{{total_scode}}分</text>
				</view>
				<view slot="canvas">
					<canvas class="CanvasBox strokeCanvas" canvas-id="BgId"></canvas>
					<canvas class="CanvasBox trailCanvas" canvas-id="InId"></canvas>
				</view>
			</iCircle>
			<view class="flex-sub flex-direction flex">
				<text class="text-bold text-lg">{{success_rate || 0}}%</text>
				<text class="text-df text-grey margin-top">正确率</text>
			</view>
		</view>
		<text class="info-title">
			多种题型综合
		</text>
		<view class="flex flex-wrap">
			<view :class="['border-info3','flex-sub',data.is_true==0?'bg-red':'bg-green']" v-for="(data,index) in itemList" :key="index"
			 @click="goToErrDetail(data.qa_id)">
				{{index+1}}
			</view>
		</view>
		<view class="flex bottom-fix">
			<text class="bg-blue flex-sub text-white" @click="goToDetail">全部解析</text>
			<text class="bg-purple flex-sub text-white" @click="goToStaillDetail">继续练习</text>
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
				list: [],
				success_scode: '',
				success_rate: '',
				itemList: [],
				total_scode: '',
				title: '',
				subTitle: '',
				pages: ''
			}
		},
		onLoad: function(option) {
			this.list = uni.getStorageSync("taskdetail_report");
			if (option.title && option.title != 'undefined') {
				this.title = option.title;
			}
			this.subTitle = option.subTitle;
			this.pages = option.pages;
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
		mounted() {
			this.add();
			this.getDetail();
		},
		methods: {
			add() {
				this.percent += 1;
			},
			getDetail() {
				let id = uni.getStorageSync('customer_id');
				// 获取做题列表
				uni.request({
					url: config.url + '/app/qa/customer/edit',
					method: "POST",
					data: {
						customer_id: id,
						qa_list: this.list
					},
					success: (res) => {
						this.itemList = res.data.data.item_list;
						this.success_rate = res.data.data.success_rate;
						this.success_scode = res.data.data.success_scode;
						this.total_scode = res.data.data.total_scode;
					}
				});
			},
			goToDetail() {
				uni.setStorageSync("report_alldetail", this.itemList)
				uni.navigateTo({
					url: '/pages/allDetail/allDetail',
				});
			},
			goToErrDetail(qa_id) {
				uni.navigateTo({
					url: '../errDetail/errDetail?qa_id=' + qa_id
				});
			},
			goToStaillDetail() {
				let id = uni.getStorageSync('customer_id');
				// 获取做题列表
				uni.request({
					url: config.url + '/app/qa/list?special_work=' + this.title + '&epoint=' + this.subTitle + '&pageindex=' + (
						Number(this.pages) + 1), //仅为示例，并非真实接口地址。
					data: {},
					success: (res) => {
						if (res.data.errcode == 0 && res.data.data.length > 0) {
							if (this.title == "写作" || this.title == "翻译") {
								uni.navigateTo({
									url: '../englishDetail/englishDetailOther?id=' + this.topicid + '&title=' + this.title + '&subTitle=' +
										this.subTitle +
										'&pages='+ (Number(this.pages) + 1)+'&showdetail=false'
								});
							} else if (this.title == "完形填空" || this.title == "阅读理解") {
								uni.navigateTo({
									url: '../englishDetail/englishDetail?id=' + this.topicid + '&title=' + this.title + '&subTitle=' + this.subTitle +
										'&pages='+ (Number(this.pages) + 1)+'&showdetail=false'
								});
							} else {
								uni.navigateTo({
									url: '../taskDetail/taskDetail?id=' + this.topicid + '&title=' + this.title + '&subTitle=' + this.subTitle +
										'&pages=' + (Number(this.pages) + 1) + '&showdetail=false'
								});
							}
						} else {
							uni.showToast({
								title: '恭喜您，已经学完了本节课程',
								duration: 2000,
								icon:"none",
								complete: function() {
									setTimeout(function() {
										uni.switchTab({
											url: '../task/task',
										});
									}, 2000)
								}
							});
						}
					}
				});
			}
		}
	}
</script>

<style scoped lang="scss">
	.box {
		/* margin-top: var(--status-bar-height); */
		flex: 1;

	}

	.view_box {
		padding: 30rpx;
		width: 100%;
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

	.middeFont {
		color: #000;
		font-size: 24rpx;
		display: block;
	}

	.border-info3 {
		min-width: 100rpx;
		height: 100rpx;
		font-size: 30rpx;
		line-height: 100rpx;
		border-radius: 50%;
		text-align: center;
		font-weight: bold;
		color: #fff;
		max-width: 100rpx;
		margin: 40rpx 10rpx 50rpx 34rpx;
	}

	.info-title {
		margin-left: 30rpx;
	}

	.bottom-fix {
		position: fixed;
		bottom: 0px;
		width: 100%;
		height: 90rpx;
		line-height: 90rpx;
		font-size: 26rpx;
		text-align: center;
	}
</style>
