<template>
	<view class="entaskDetail">
		<!-- <uni-countdown :showDay="false" :day="0" :hour="0" :minute="0" :second="0">
		</uni-countdown> -->
		<view class="advertisement">
			<scroll-view :scroll-y="true" bindscrolltolower="scrollbot">
				<!-- <web-view src="/hybrid/html/local.html" style="height: 200rpx;top:100rpx" @message="getMessage"></web-view> -->
				<!-- <u-parse :content="titleContent" :clickHandler="clickHandler" />
				<view class="uni-common-mt" style="background:#FFF; padding:20rpx;">
					<rich-text :nodes="nodes"></rich-text>
					<view v-html="titleContent" @click="clicki"></view>
				</view> -->
				<view v-for="(item,index) in titleContent" :key="index" :class="[item.indexOf('<')>=0?'block':'disInline','margin-left-sm',clickIndex==index?'text-blue':'']"
				 @click="titleIndex(item,index)" v-html="item">
					<!-- <u-parse :content="" /> -->
				</view>
				<!-- <text @click="getIndex">
					{{titleContent}}
				</text> -->
			</scroll-view>
		</view>
		<swiper class="swiper" :indicator-dots="indicatorDots" @change="objectChange" :autoplay="false" :interval="2000"
		 :duration="500" :current="curryIndex">
			<swiper-item v-for="(item, index) in list" :key="index">
				<scroll-view :scroll-y="true" bindscrolltolower="scrollbot">
					<view class="ans-list">
						<view class="ans-item" v-for="(subitem, subindex) in item" :key="subindex" @click="setChoose(index,subindex,subitem,item.type)">
							<view class="item-left flex-sub">
								<!-- <view v-if="" :class="['border-info',subitem.isChoose?'border-info2':'']">{{subitem.option}}</view> -->
								<view :class="['border-info',subitem.isChoose?'border-info2':'']">{{subitem.option}}</view>
							</view>
							<view class="item-right">
								<u-parse :content="subitem.content" />
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
				<view class="border-info3 flex-sub" v-for="(item,index) in Object.keys(list)" :key="index">
					{{index+Number(1)}}
				</view>
			</view>
		</neil-modal>

	</view>
</template>

<script>
	var wv; //计划创建的webview
	import uniCountdown from "@/components/uni-countdown/uni-countdown.vue"
	import config from '../../config.js';
	import neilModal from '@/components/neil-modal/neil-modal.vue';
	import uParse from '@/components/gaoyia-parse/parse.vue'

	// import mockinfo from './mockinfo.json'
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
				item_list: [],
				ansList: [],
				isShow: false,
				showdetail: false,
				isTopic: false,
				clientHeight: 0,
				fileList: [],
				titleContent: [],
				clickIndex: -1,
				chooseList: [],
				nodes: [{
					name: 'div',
					attrs: {
						class: 'div-class',
						style: 'line-height: 60px; color: red; text-align:center;'
					},
					children: [{
						type: 'text',
						text: 'Hello&nbsp;uni-app!',
						click: function() {
							console.log(2)
						}
					}]
				}],
			}
		},
		getIndex() {

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
		onReady() {
			// #ifdef APP-PLUS
			var currentWebview = this.$mp.page.$getAppWebview() //获取当前页面的webview对象
			setTimeout(function() {
				wv = currentWebview.children()[0]
				wv.setStyle({
					top: 150,
					height: 300
				})
			}, 1000); //如果是页面初始化调用时，需要延时一下
			// #endif
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
			titleIndex(data, index) {
				if (data.indexOf('${') > 0) {
					this.clickIndex = index;
				}
				this.chooseList.forEach((data3, index3) => {
					if (data3 === data) {
						this.curryIndex = index3;
					}
				})
			},
			objectChange(e) {
				this.curryIndex = e.detail.current;
				this.titleContent.forEach((data3, index3) => {
					if (data3 === this.chooseList[e.detail.current]) {
						this.clickIndex = index3;
					}
				})
			},
			getMessage(e) {
				uni.showModal({
					content: JSON.stringify(e.detail),
					showCancel: false
				})
			},
			clickHandler(e) {
				console.log(e);
			},
			getDetail() {
				this.titleContent = [];
				let id = uni.getStorageSync('customer_id');
				let url = ''
				if (this.isTopic) {
					url = config.url + '/app/qa/list?topicid=' + this.topicid + '&pageindex=' + this.pages;
				} else {
					url = config.url + '/app/qa/list?special_work=' + this.title + '&epoint=' + this.subTitle + '&pageindex=' + this.pages;
				}
				// 获取做题列表
				uni.request({
					// url: config.url+'/app/qa/list?special_work='+this.title+'&epoint='+this.subTitle+'&pageindex='+this.pages,
					url,
					data: {},
					success: (res) => {
						let c = res.data.data[0].title.replace(/<(?!\/?br\/?.+?>|\/?img.+?>)[^<>]*>/gi, '');
						c.split(' ').forEach((data) => {
							if (data.indexOf('<br/>') > 0) {
								let newLabel = data.split('<br/>').join('#<view class="block"></view>#');
								let b = newLabel.split('#');
								this.titleContent = this.titleContent.concat(b);
							} else {
								this.titleContent.push(data);
							}
						})
						this.list = res.data.data[0].item_list;
						this.titleContent.some((data2, index2) => {
							if (data2.indexOf('${') > 0) {
								this.clickIndex = index2;
								return true;
							}
						});
						this.titleContent.forEach((data, index) => {
							if (data.indexOf('${') > 0) {
								this.chooseList.push(data);
							}
						})
					}
				});
			},
			setChoose(index, subindex, subitem, type) {
				if (this.showdetail) return false;
				this.list[index].map((data2) => {
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

				this.list[index][subindex]['isChoose'] = true;
				if (this.ansList.length == Object.keys(this.list).length) {
					this.isShow = true;
				} else {
					this.curryIndex = this.curryIndex + 1;
					this.clickIndex = this.clickIndex + 1;
				}


			},
			bindBtn(type) {
				uni.navigateTo({
					url: '../report/report?id=' + this.topicid + '&title=' + this.title + '&subTitle=' + this.subTitle + '&pages=' +
						this.pages + '&showdetail=false' + '&list=' + JSON.stringify(this.ansList)
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

	uni-swiper {
		height: auto;
	}

	uni-swiper .uni-swiper-wrapper {
		overflow-x: hidden;
		overflow-y: scroll;
	}
	.swiper{
		margin-top: 20rpx;
		height: 200px;
	}
	.advertisement {
		width: 100%;
		min-height: 40%;
		max-height: 40%;
		padding: 20upx;
		overflow-y: scroll;
		border-bottom: 4upx #555555 solid;
	}

	.entaskDetail {
		width: 100%;
		padding: 0rpx 20rpx;
		background-color: #fff;
		height: 100%;
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
