<template>
	<view class="entaskDetail">
		<view class="advertisement">
			<view v-html="ttitle" style="	overflow-x: hidden;height: 1000upx;	overflow-y: scroll;">

			</view>
		</view>
		<view style="bottom: 0px;position: absolute;height: 210px;padding: 10px;background-color: #f5f5f5;width: 100%;">
			<textarea placeholder="编辑答案........" class="textarea" maxlength="10000" v-model="content"></textarea>
			<view class="save" @click="save">保 存</view>
		</view>
	</view>
</template>

<script>
	var wv;
	import config from '../../config.js';
	export default {
		components: {},
		data() {
			return {
				ttitle: '',
				qa_id: '',
				content: '',
				qa_index: 0,
			}
		},
		onShow() {
			this.getDetail(this.qa_index);
			var that = this
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
			getDetail(index) {
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
						this.ttitle = res.data.data[index].title;
						this.qa_id = res.data.data[index].innerid;
					}
				});
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
	.advertisement {
		width: 100%;
		background-color: #fdfdfd;
		padding: 20rpx;
	}

	.entaskDetail {
		width: 100%;
		background-color: #fff;
	}

	.textarea {
		width: 100%;
		background-color: white;
		border: 1rpx solid #f1f1f1;
		border-radius: 2px;
		min-height: 100rpx;
		font-size: 14px;
		// margin-bottom:10px;
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
</style>
