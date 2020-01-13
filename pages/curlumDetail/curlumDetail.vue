<template>
	<view class="curlum-detail">
		<video id="myVideo" :src="vedio_url" @error="videoErrorCallback" controls @timeupdate="videoTimeupdate"></video>

		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name"></sun-tab>
		<!-- 简介 -->
		<view class="info" v-if="index==0">
			<text class="title">{{curlumName}}</text>
			<text class="subtitle text-darkGrey">{{curlumSubInfo}}</text>
			<view class="text-darkGrey info-type flex justify-between">
				<text class="">{{summary}}</text>
				<view class="">
					<text class="cuIcon-attentionfavorfill text-darkGrey text-lg"></text>
					<text class="margin-left-sm">{{buy_count}}</text>
				</view>
			</view>
			<text class="price">
				{{old_price==0?'免费':old_price+'元'}}
			</text>
			<view class="content-img">
				<image :src="content" mode="widthFix" style="width: 100%;height: 100%;"></image>
			</view>
		</view>
		<!-- 目录 -->
		<view class="directory" v-if="index==1">
			<uni-collapse @change="change" v-for="firitem in parintList" :key="firitem.innerid">
				<uni-collapse-item :title="firitem.title" :show-animation="true">
					<view class="item flex" v-for="(item,subindex) in firitem.subList" :key="item.innerid" @click="setPlay(firitem,item,subindex)">
						<view class="item-left">
							<text :class="[item.isPlaying?'text-blue':'text-darkGrey','cuIcon-videofill','text-xxxl']"></text>
						</view>
						<view :class="[ item.isPlaying?'text-blue':'', 'item-right','text-sm']" style="flex:8">
							<text class="text-df margin-top-xs block">{{item.title}}</text>
							<view class="item-time">
								<text>{{item.allTime}}</text>
								<text class="text-blue margin-left-lg" v-if="item.study_speed!==''">上次观看至{{item.study_speed}}</text>
							</view>
						</view>
					</view>
				</uni-collapse-item>
			</uni-collapse>
		</view>
		<!-- 评论 -->
		<view class="comment" v-if="index==2">
			<!-- <text class="title">精彩评论({{goodlist.length}})</text>
			<comment :list="goodlist" @thumbsGoodUp="thumbsGoodUp"></comment> -->
			<mescroll-uni :down="downOption" @down="downCallback" :up="upOption" @up="upCallback" :fixed="false" bottom="20" @init="mescrollInit">
				<view v-if="newlist.length!=0">
					<text class="title">最新评论({{newlist.length}})</text>
					<comment :list="newlist" @thumbsListUp="thumbsListUp"></comment>
				</view>
			</mescroll-uni>
			<view v-if="newlist.length==0">
				<text class="text-center">暂无评论</text>
			</view>

		</view>
		<view class="comment-post" v-if="is_sgin==1">
			<input type="text" v-model="commentInfo" placeholder="请输入你的评论" />
			<text class="post-button" @click="postCommit">发布</text>
		</view>
		<view class="comment-post" v-if="is_sgin==0">
			<text class="post-button2" @click="reportInfo">立即报名</text>
		</view>
	</view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import comment from '@/components/commoent/uni-comment.vue';
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item-curlum/uni-collapse-item-curlum.vue'
	import config from '../../config.js';
	import MescrollUni from "@/components/mescroll-uni/mescroll-uni.vue";
	export default {
		components: {
			sunTab,
			comment,
			uniCollapse,
			uniCollapseItem,
			MescrollUni
		},
		data() {
			return {
				curlumName: '',
				curlumSubInfo: '',
				old_price: '',
				curlumType: '',
				commentInfo: '',
				index: 0,
				PlayNum: 0,
				vedio_url: '',
				buy_count: '',
				summary: '',
				content: '',
				mescroll: null,
				parintList: [],
				newlist: [],
				downOption: {
					use: false,
					auto: false
				},
				upOption: {
					use: true,
					auto: true,
					isBounce: false,
					page: {
						num: 0,
						size: 10
					},
					noMoreSize: 1,
					offset: 80
				},
				tabObjectList: [ //对象数组赋值
					{
						name: '简介',
						value: 0
					},
					{
						name: '目录',
						value: 1
					},
					{
						name: '评论',
						value: 2
					}
				],
				course_id: '',
				is_sgin: '',
				book_id: '',
				videoContext: '',
				currentTime: ''
			}
		},
		onShow() {
			if(this.is_sgin==0){
				this.getMathDetail();
				this.tabObjectList= [ //对象数组赋值
					{
						name: '简介',
						value: 0
					}
				]
			}else{
				this.getDetail();
				this.getMathDetail();
				this.getCommentList();
				this.tabObjectList=[ //对象数组赋值
					{
						name: '简介',
						value: 0
					},
					{
						name: '目录',
						value: 1
					},
					{
						name: '评论',
						value: 2
					}
				]
			}
			
			
		},
		onReady: function(res) {
			// #ifndef MP-ALIPAY
			this.videoContext = uni.createVideoContext('myVideo');
			// #endif
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			this.course_id = option.course_id;
			this.is_sgin = option.is_sgin;
		},
		onUnload: function() {
			console.log(this.book_id);
			debugger;
			if(this.book_id){
				
			}
			let userid = uni.getStorageSync('customer_id');
			uni.request({
				url: config.url + '/app/course/book/play/speed',
				method: "POST",
				data: {
					'course_id': this.course_id,
					'customer_id': userid,
					'book_id': this.book_id,
					'speed_time': this.currentTime,
				},
				success: (res) => {
				}
			});
		},
		methods: {
			upCallback(mescroll) {
				this.getListDataFromNet(mescroll.num, (curPageData) => {
					mescroll.endSuccess(curPageData.length);
					if (mescroll.num == 1) this.list2 = []; //如果是第一页需手动制空列表
					this.list2 = this.list2.concat(curPageData); //追加新数据
				}, () => {
					mescroll.endErr();
				})
			},
			getListDataFromNet(pageNum, successCallback, errorCallback) {
				setTimeout(() => {
					try {
						this.getList(pageNum - 1, successCallback)
					} catch (e) {
						//联网失败的回调
						errorCallback && errorCallback();
					}
				}, 300)
			},
			downCallback(mescroll){
				mescroll.endSuccess()
			},
			mescrollInit(mescroll) {
				this.mescroll = mescroll;
			},
			videoTimeupdate(e) {
				this.currentTime = e.detail.currentTime;
			},
			videoErrorCallback: function(e) {
				// console.log(e);
				// uni.showModal({
				// 	content: e.target.errMsg,
				// 	showCancel: false
				// })
			},
			postCommit() {
				if (this.commentInfo == '') {
					return false;
				}
				let userid = uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url + '/app/comment/add',
					method: "POST",
					data: {
						"course_id": this.course_id,
						"customer_id": userid,
						"content": this.commentInfo
					},
					success: (res) => {
						if (res.data.errcode == 0) {
							uni.showToast({
								title: '评论成功'
							});
							this.commentInfo = ''
							this.getCommentList();

						}
					}
				});
			},
			getCommentList() {
				let userid = uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url + '/app/comment/list/' + this.course_id + '?pageindex=0',
					success: (res) => {
						if (res.data.errcode == 0) {
							this.newlist = res.data.data;
						} else {

						}
					}
				});
			},
			reportInfo() {
				let userid = uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url + '/app/sgincourse/sgin',
					method: "POST",
					data: {
						"course_id": this.course_id,
						"customer_id": userid
					},
					success: (res) => {
						console.log(res)
						if (res.data.errcode == 0) {
							this.is_sgin = 1;
							uni.showToast({
								title: '报名成功'
							});
							// uni.redirectTo({
							//     url: '../curlumDetail/curlumDetail?course_id='+this.course_id+'&is_sgin=1'
							// });
						} else {
							uni.showToast({
								title: res.data.errmsg
							})
						}

					}
				});

			},
			objectChange(e) {
				this.index = e.tab.value;
			},
			change(e) {},
			setPlay(firitem, item, index) {
				let c = [];
				this.parintList.forEach((data) => {
					data.subList.forEach((subdata) => {
						subdata.isPlaying = false;
					})
				});
				item.isPlaying = true;
				if (item.vedio_url == '') {
					uni.showToast({
						title: '暂无视频'
					});

				} else {
					this.vedio_url = item.vedio_url;
					this.videoContext.play();
				}
				this.book_id = item.innerid;
			},
			thumbsListUp(data) {
				console.log(data);
				let userid = uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url + '/app/praise/add',
					method: "POST",
					data: {
						"customer_id": this.course_id,
						"comment_id": data.item.innerid
					},
					success: (res) => {
						if (res.data.errcode == 0) {
							this.newlist[data.index]['isLike'] = true;
							this.newlist[data.index]['praise_count'] = ++this.newlist[data.index]['praise_count'];
							uni.showToast({
								title: '点赞成功'
							});
						} else {
							uni.showToast({
								title: res.data.errmsg
							})
						}
					}
				});
			},
			// 获取课程详情
			getMathDetail() {
				let userid = uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url + '/app/course/detail/' + this.course_id + '/' + userid, //仅为示例，并非真实接口地址。
					data: {},
					success: (res) => {
						if (res.data.errcode == 0) {
							// console.log(res.data.data)
							this.summary = res.data.data.summary;

							this.old_price = res.data.data.price
							this.buy_count = res.data.data.buy_count;
							this.curlumSubInfo = res.data.data.sub_tilte;
							this.content = res.data.data.content;
							uni.setNavigationBarTitle({
								title: res.data.data.title
							});
						} else {
							uni.showToast({
								title: res.data.errmsg
							})
						}

					}
				});
			},
			// 获取目录
			getDetail() {
				let userid = uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url + '/app/course/book/list/' + this.course_id + '/' + userid, //仅为示例，并非真实接口地址。
					data: {},
					success: (res) => {
						if (res.data.errcode == 0) {
							if (res.data.data.length > 0) {
								let newList = [];
								res.data.data.map(data => {
									if (data.parent_id == this.course_id) {
										data['subList'] = [];
										newList.push(data)
									}
								})
								res.data.data.map((data, index) => {
									newList.map((subdata, subindex) => {
										if (subdata.innerid == data.parent_id) {
											data.isPlaying = false;
											newList[subindex]['subList'].push(data);
										}
									})
								})
								this.parintList = newList;
								this.vedio_url = res.data.data[0].vedio_url;
								this.book_id = res.data.data[0].innerid;
							} else {
								// uni.showToast({
								// 	title: '暂无课程'
								// })
							}

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
	.curlum-detail {
		background-color: #fff;
		width: 100%;

		video {
			width: 100%
		}

		.info {
			border-bottom: 2rpx #ccc solid;
			font-size: 24rpx;

			.title {
				font-weight: bold;
				font-size: 34rpx;
				margin: 20rpx 30rpx;
				display: block;
			}

			.subtitle {
				font-size: 28rpx;
				margin: 20rpx 30rpx;
			}

			.info-type {
				font-size: 24rpx;
				margin: 20rpx 30rpx;
			}

			.price {
				color: red;
				font-size: 34rpx;
				display: block;
				margin: 20rpx 30rpx;
			}

			.content-img {
				// padding: 40rpx;
			}
		}
		.directory {
			padding-bottom: 100rpx;
			.item {
				width: 100%;
				.item-left {
					margin: 34rpx 30rpx 0rpx;
					width: 56rpx;
					height: 56rpx;
					max-width: 56rpx;
				}
				.item-right {
					border-bottom: 2rpx #ccc solid;
					padding-top: 30rpx;
					padding-bottom: 20rpx;
					.item-time {
						margin-top: 30rpx;
					}
				}
			}
		}
		.comment {
			display: inline-block;
			padding-bottom: 110rpx;
			width: 100%;
			.title {
				font-size: 32rpx;
				margin: 16rpx 0px 10rpx 26rpx;
				display: inline-block;
			}

		}

		.comment-post {
			position: fixed;
			bottom: 0px;
			height: 100rpx;
			display: flex;
			width: 100%;
			background: #fff;
			z-index: 1;

			input {
				flex: 3;
				margin: 20rpx 0rpx 0px 30rpx;
				background-color: #eee;
				text-indent: 20rpx;
				font-size: 30rpx;
				height: 60rpx;
				color: #858B9C;
				border-radius: 10rpx;
			}

			.post-button {
				flex: 1;
				margin: 20rpx 30rpx 0px 30rpx;
				border-radius: 40rpx;
				font-size: 28rpx;
				text-align: center;
				color: #fff;
				height: 60rpx;
				line-height: 60rpx;
				background-color: blue;
			}

			.post-button2 {
				flex: 1;
				margin: 20rpx 100rpx 0px 100rpx;
				border-radius: 40rpx;
				font-size: 28rpx;
				text-align: center;
				color: #fff;
				height: 70rpx;
				line-height: 70rpx;
				background-color: blue;
			}
		}



	}
</style>
