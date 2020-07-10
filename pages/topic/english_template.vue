<template>
	<view class="entaskDetail">
		<view class="flex flex-direction" style="width: 100%;">
			<view class="advertisement" :style="{height:clientHeight?clientHeight+'px':'auto'}">
				<label v-for="(item,index) in titleContent" :key="index" :class="[(curryIndex*2+1)==index?'sel_item':'','',clickIndex==index?'':'']"
				 @click="titleIndex(item,index)" v-html="item">
				</label>
			</view>
			<swiper class="swiper" :indicator-dots="indicatorDots" @change="objectChange" :autoplay="false" :interval="2000"
			 :duration="500" :current="curryIndex" :style="{height:clientHeight?clientHeight+'px':'auto'}">
				<swiper-item v-for="(item, index) in list" :key="index">
					<scroll-view :scroll-y="true" bindscrolltolower="scrollbot">
						<view class="ans-list">
							<view v-if="epoint!='写作' && epoint!='翻译'" class="ans-item" v-for="(subitem, subindex) in item" :key="subindex"
							 @click="setChoose(index,subindex,subitem,item.type)">

								<view class="item-left flex-sub" v-if="subitem.option">
									<view :class="['border-info',subitem.isChoose?'border-info2':'']">{{subitem.option}}</view>
								</view>
								<view class="item-right">
									<u-parse :content="subitem.content" />
								</view>


							</view>
							<view class="ans-item" v-if="epoint=='写作' || epoint=='翻译'" style="height: 200px;" >
								<view style="bottom: 0px;position: absolute;height: 250px;padding: 10px;background-color: #f5f5f5;width: 100%;">
									<textarea placeholder="编辑答案........" class="textarea" maxlength="10000" v-model="content"></textarea>
									<view class="save" @click="save">保 存</view>
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
		</view>
		<neil-modal :show="isShow" @close="closeModal" title="答题卡" @cancel="bindBtn('cancel')" confirmText="交卷并查看结果" @confirm="bindBtn('confirm')">
			<text class="modal_info">
				多种题型综合
			</text>
			<view class="flex flex-wrap">
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
	export default {
		components: {
			uniCountdown,
			neilModal,
			uParse
		},
		data() {
			return {
				ttitle: '',
				title_content: "",
				content:"",
				indicatorDots: false,
				curryIndex: 0,
				type: '',
				list: [],
				item_list: [],
				ansList: [],
				height: 0,
				isShow: false,
				showdetail: false,
				isTopic: false,
				clientHeight: 0,
				clientHeight2: 0,
				fileList: [],
				titleContent: [],
				clickIndex: -1,
				chooseList: [],
				qa_index: 0,
				epoint: "",
				classlist: ['sel_item', 'sel_item'],
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
					this.clientHeight = res.windowHeight / 2;
				}
			});
		},
		onReady() {
			// var currentWebview = this.$mp.page.$getAppWebview()
			// setTimeout(function() {
			// 	wv = currentWebview.children()[0]
			// 	wv.setStyle({
			// 		top: 150,
			// 		height: 300
			// 	})
			// }, 1000);
		},
		onLoad: function(option) {
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
			objectChange_new(e) {
				this.curryIndex = e.detail.current;
			},
			objectChange(e) {
				const query = uni.createSelectorQuery()
				query.select('.sel_item').boundingClientRect()
				query.selectViewport().scrollOffset()
				query.exec(function(res) {
					uni.pageScrollTo({
						scrollTop: res[0].top,
						duration: 300
					});
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
					url,
					data: {},
					success: (res) => {
						this.list = res.data.data[this.qa_index].item_list;
						this.epoint = res.data.data[this.qa_index].epoint;

						let c = res.data.data[this.qa_index].title;
						c = c.replace(/<img/g, '<img style="width:100%;"');

						for (let i = 1; i <= 21; i++) {
							let s_str = c.split('${' + i + '}')
							if (s_str.length > 1) {
								if (i == 1) {
									this.titleContent.push('(' + this.epoint + ')' + s_str[0])
								} else {
									this.titleContent.push(s_str[0])
								}
								this.titleContent.push('__' + i + '__')
								c = s_str[1];
							} else {
								this.titleContent.push(s_str[0])
								break;
							}
						}
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
				uni.setStorageSync("taskdetail_report", this.ansList)
				uni.navigateTo({
					url: '../report/report?id=' + this.topicid + '&title=' + this.title + '&subTitle=' + this.subTitle + '&pages=' +
						this.pages + '&showdetail=false' + '&list=' + JSON.stringify(this.ansList)
				});
			},
			closeModal() {
				this.isShow = false
			},
			save() {
				let id = uni.getStorageSync('customer_id');
				let _self = this;
				// 获取做题列表
				uni.request({
					url: config.url + '/app/qa/english/customer/edit',
					method: "POST",
					data: {
						customer_id: id,
						qa_id: this.qa_id,
						content: this.content
					},
					success: (res) => {
						if (res.data.data == 1) {
							uni.showModal({
								title: '保存成功，是否进入【下一题】？',
								confirmText: '下一题',
								success: function() {
									_self.content = '';
									_self.qa_index += 1;
									_self.getDetail(_self.qa_index);
								}
							})
						}
			
					}
				});
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

	// p img {
	// 	width: 100%;
	// }

	.swiper {
		margin-top: 20rpx;
		padding: 30rpx;
	}

	.advertisement {
		width: 100%;
		overflow-y: scroll;
		border-bottom: 4upx #555555 solid;
		background-color: #fdfdfd;
		padding: 44rpx;
		line-height: 44upx;
	}

	.entaskDetail {
		width: 100%;
		background-color: #fff;
		height: 100%;
		display: flex;

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
		min-width: 80rpx;
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

	.sel_item {
		color: blue;
		font-family: fantasy;
	}

	.no_sel_item {
		color: #0077AA;
	}
	.save {
		height: 40px;
		border: 1px solid green;
		background-color: green;
		color: white;
		margin-top: 5px;
		text-align: center;
		line-height: 40px;
		border-radius: 5px;
		font-size: 16px;
	}
	
	.textarea {
		width: 100%;
		background-color: white;
		border: 1rpx solid #f1f1f1;
		border-radius: 2px;
		min-height: 100rpx;
		font-size: 14px;
		margin-top: 10px;
	}
</style>
