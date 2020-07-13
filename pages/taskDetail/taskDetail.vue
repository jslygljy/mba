<template>
	<view class="taskDetail">
		<view class="flex justify-between">
			<uni-countdown :showDay="false" :day="0" :hour="0" :minute="0" :second="0">
			</uni-countdown>
			<view class="icons" @click="likeClick">
				<text :class="['text-xxl',haslike?'text-red cuIcon-likefill':'cuIcon-like']"></text>
			</view>
		</view>
		<swiper class="swiper" :style="{height:clientHeight?clientHeight+'px':'auto'}" :indicator-dots="indicatorDots"
		 @change="objectChange" :autoplay="false" :interval="2000" :duration="500" :current="curryIndex">
			<swiper-item v-for="(item, index) in list" :key="index">
				<scroll-view :scroll-y="true" :style="{height:clientHeight?clientHeight+'px':'auto'}" bindscrolltolower="scrollbot">
					<view class="header">
						<text>{{item.ttitle}}</text>
						<view class="right-progress">
							<text class="text-blue">{{curryIndex+1}}</text>
							/
							<text>{{list.length}}</text>
						</view>
					</view>
					<view class="content">
						<view>
							<text>({{item.type==1? '单选题':'多选题'}})</text>
							<!-- <u-parse :content=""/> -->
							<view v-html="item.title"></view>
						</view>
					</view>
					<view class="ans-list">
						<view class="ans-item" v-for="(subitem, subindex) in item.item_list" :key="subindex" @click="setChoose(index,subindex,subitem,item.type)">
							<view class="item-left flex-sub">
								<!-- <view v-if="" :class="['border-info',subitem.isChoose?'border-info2':'']">{{subitem.option}}</view> -->
								<view :class="['border-info',subitem.isChoose?'border-info2':'']">{{subitem.option}}</view>
							</view>
							<view class="item-right">
								<!-- <u-parse :content="subitem.content"/> -->
								<view v-html="subitem.content"></view>
							</view>
						</view>
					</view>
					<view class="" v-if="showdetail">
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
				</scroll-view>
			</swiper-item>
		</swiper>
		<neil-modal :show="isShow" @close="closeModal" title="答题卡" @cancel="bindBtn('cancel')" confirmText="交卷并查看结果" @confirm="bindBtn('confirm')">
			<text class="modal_info">
				多种题型综合
			</text>
			<view class="flex">
				<view class="border-info3 flex-sub" v-for="(item,index) in list" :key="index">
					{{index+1}}
				</view>
			</view>
		</neil-modal>

	</view>
</template>

<script>
	import uniCountdown from "@/components/uni-countdown/uni-countdown.vue"
	import config from '../../config.js';
	import neilModal from '@/components/neil-modal/neil-modal.vue';
	import uParse from '@/components/feng-parse/parse.vue'
	export default {
		components: {
			uniCountdown,
			neilModal,
			uParse
		},
		data() {
			return {
				ttitle: '',
				indicatorDots: false,
				curryIndex: 0,
				type: '',
				list: [],
				haslike: false,
				item_list: [],
				ansList: [],
				isShow: false,
				showdetail: false,
				isTopic: false,
				clientHeight: 0,
				innerid: '',
				article: '<p>html代码，具体参见https://github.com/gaoyia/parse/tree/1.0.7/parse-demo中的demo</p>'
			}
		},
		onShow() {
			this.getDetail();
			var that = this
			wx.getSystemInfo({
				success: (res) => {
					this.clientHeight = res.windowHeight
				}
			});
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			this.topicid = option.topicid;
			this.title = option.title;
			this.subTitle = option.subTitle;
			this.pages = option.pages;
			this.isTopic = option.isTopic;
			this.showdetail = option.showdetail === "false" ? false : true;
		},
		methods: {
			objectChange(e) {
				this.curryIndex = e.detail.current;
			},
			getDetail() {
				let id = uni.getStorageSync('customer_id');
				let url = ''
				if (this.isTopic) {
					url = config.url + '/app/qa/list?topicid=' + this.topicid + '&pageindex=' + this.pages + '&customer_id=' + id;
				} else {
					url = config.url + '/app/qa/list?special_work=' + this.title + '&epoint=' + this.subTitle + '&pageindex=' + this.pages +
						'&customer_id=' + id;
				}
				// 获取做题列表
				uni.request({
					// url: config.url+'/app/qa/list?special_work='+this.title+'&epoint='+this.subTitle+'&pageindex='+this.pages,
					url,
					data: {},
					success: (res) => {
						if (res.data.errcode == 0 && res.data.data.length > 0) {
							res.data.data.map((data) => {
								data.item_list.map((data2) => {
									data2.isChoose = false;
								})
							})
							this.list = res.data.data;
							this.haslike = res.data.data[0].is_collect;
							this.innerid = res.data.data[0].innerid;
						} else {
							uni.showToast({
								title: '恭喜您，已经学完了本节课程',
								duration: 2000,
								icon:"none",
								complete: function() {
									setTimeout(function() {
										uni.navigateBack()
									}, 2000)
								}
							});

						}
					}
				});
			},
			likeClick() {
				let id = uni.getStorageSync('customer_id');
				if (!this.haslike) {
					uni.request({
						url: config.url + '/app/collect/add',
						method: "POST",
						data: {
							customer_id: id,
							qa_innerid: this.innerid
						},
						success: (res) => {
							if (res.data.errcode == 0) {
								this.haslike = 1;
							}
						}
					});
				} else {
					uni.request({
						url: config.url + '/app/collect/del',
						method: "POST",
						data: {
							customer_id: id,
							qa_innerid: this.innerid
						},
						success: (res) => {
							if (res.data.errcode == 0) {
								this.haslike = 0;
							}
						}
					});
				}

			},
			setChoose(index, subindex, subitem, type) {
				if (this.showdetail) return false;
				this.list[index].item_list.map((data2) => {
					data2.isChoose = false;
				})

				if (!this.ansList[index]) {
					this.ansList.push({
						qa_id: subitem.qa_id,
						is_true: subitem.is_true,
						answer: subitem.option
					});
				} else {
					this.ansList[index].qa_id = subitem.qa_id;
					this.ansList[index].is_true = subitem.is_true;
					this.ansList[index].answer = subitem.option;
				}

				this.list[index].item_list[subindex]['isChoose'] = true;
				if (this.ansList.length == this.list.length) {
					this.isShow = true;
				} else {
					this.curryIndex = this.curryIndex + 1
				}


			},
			bindBtn(type) {
				uni.setStorageSync("taskdetail_report", this.ansList)
				uni.navigateTo({
					url: '/pages/report/report?id=' + this.topicid + '&title=' + this.title + '&subTitle=' + this.subTitle +
						'&pages=' + this.pages + '&showdetail=false'
				});
			},
			closeModal() {
				this.isShow = false
			}
		}
	}
</script>

<style scoped lang="scss">
	@import url("../../components/gaoyia-parse/parse.css");

	uni-swiper-item {
		overflow-y: scroll;
		overflow-x: hidden;
	}

	.swiper {
		display: flex;
		flex: 1 1 auto;
	}

	uni-swiper {
		height: auto;
	}

	uni-swiper .uni-swiper-wrapper {
		overflow-x: hidden;
		overflow-y: scroll;
	}

	.taskDetail {
		display: flex;
		flex-direction: column;
		width: 100%;
		padding: 0rpx 20rpx;
		background-color: #fff;

		.header {
			display: flex;
			justify-content: space-between;
			font-size: 26rpx;
			height: 80rpx;
			line-height: 80rpx;
			border-bottom: 2rpx #ddd solid;
			margin-bottom: 40rpx;
		}

		.content {
			font-size: 32rpx;
			margin-bottom: 60rpx;
			width: 100%;
		}

		.ans-item {
			display: flex;
			margin-bottom: 80rpx;

			.item-left {
				.border-info {
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

				.border-info2 {
					color: #fff;
					background-color: #0081ff;
				}
			}

			.item-right {
				font-size: 30rpx;
				flex: 5;
				line-height: 46rpx;
				align-self: center;
				margin-left: 20px;
			}
		}
	}

	.border-info3 {
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

	.modal_info {
		margin: 10rpx 10rpx 20rpx 20rpx;
		display: inline-block;
	}

	.title {
		font-weight: bold;
		font-size: 34rpx;
		padding: 10rpx;
		display: block;
		margin-top: 20rpx;
	}

	.info {
		font-size: 28rpx;
		padding: 10rpx;
		display: block;
	}
</style>
