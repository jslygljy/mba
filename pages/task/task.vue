<template>
	<view class="task-content">
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name"></sun-tab>
		<view class="list">
			<view class="item" @click="goToRead">
				<text class="cuIcon-newshotfill text-blue i"></text>
				<text>套卷练习</text>
			</view>
			<view class="item" @click="goToHistory">
				<text class="cuIcon-babyfill text-purple i"></text>
				<text>做题历史</text>
			</view>
			<view class="item" @click="goToReport">
				<text class="cuIcon-babyfill text-purple i"></text>
				<text>学习报告</text>
			</view>
		</view>
		<text class="practice-title">
			专项练习
		</text>
		<view class="collapse-info">
			<uni-collapse @change="change" v-for="item in list" :key="item.title" v-if="list.length!==0">
				<uni-collapse-item :title="item.title" :show-animation="true" :curryNum="item.have_sure" :allNum="item.arate"
				 :haveSure="item.have_sure">
				 
					<uni-list class="list-info" :key="subindex" v-for="(subitem, subindex) in item.item_list">						
						<uni-list-item 
							:title="subitem.title" :curryNum="subitem.have_sure" :allNum="subitem.arate" :haveSure="subitem.have_sure"
							@click="goToDetail(subitem,item.title)"
						>
						</uni-list-item>
					</uni-list>
				</uni-collapse-item>
			</uni-collapse>
			<view class="text-center" v-if="list.length==0">
				<text>暂无数据</text>
			</view>
		</view>

	</view>
</template>

<script>
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item/uni-collapse-item.vue'
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import config from '../../config.js';
	export default {
		components: {
			uniCollapse,
			uniCollapseItem,
			uniList,
			uniListItem,
			sunTab
		},
		data() {

			return {
				list: [],
				index: 0,
				curryindex: 1,
				tabObjectList: [ //对象数组赋值
					{
						name: '逻辑',
						value: 1
					},
					{
						name: '数学',
						value: 2
					},
					{
						name: '英语',
						value: 3
					},
					{
						name: '写作',
						value: 4
					},
					{
						name: '面试',
						value: 5
					},
					{
						name: '助力',
						value: 6
					},{
						name: '真题',
						value: 7
					},
					{
						name: '联考',
						value: 8
					}
				],
			}
		},
		onShow() {
			this.getList();
		},
		methods: {
			onClick() {
				this.list[1].data[1].subList.push({
					title: '新增',
					thumb: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png'
				})
				this.$nextTick(() => {
					this.$refs.add[1].resize()
				})
			},
			change(e) {
				console.log(e)
			},
			objectChange(e) {
				this.curryindex = e.tab.value;
				this.getList();

			},

			goToDetail(sub,title) {
				if(this.curryindex==3){
					uni.navigateTo({
						url: '../englishDetail/englishDetail?id=' + sub.topicid + '&title=' + title + '&subTitle=' + sub.title +
							'&pages=0&showdetail=false'
					});
				}else{
					uni.navigateTo({
						url: '../taskDetail/taskDetail?id=' + sub.topicid + '&title=' + title + '&subTitle=' + sub.title +
							'&pages=0&showdetail=false'
					});
				}
				// let id = uni.getStorageSync('customer_id');
				// // 获取做题列表
				// uni.request({
				// 	url: config.url + '/app/qa/list?special_work=' + title + '&epoint=' + subTitle + '&pageindex=0',
				// 	data: {},
				// 	success: (res) => {
				// 		console.log(res)
				// 		// if (res.data.data[0].qtype == 4) {
							
				// 		// } else {
							
				// 		// }
				// 	}
				// });
				// if (this.curryindex == 3) {
				// 	uni.navigateTo({
				// 		url: '../englishDetail/englishDetail?id=' + topicid + '&title=' + title + '&subTitle=' + subTitle +
				// 			'&pages=0&showdetail=false'
				// 	});
				// } else {
				// 	uni.navigateTo({
				// 		url: '../taskDetail/taskDetail?id=' + topicid + '&title=' + title + '&subTitle=' + subTitle +
				// 			'&pages=0&showdetail=false'
				// 	});
				// }
			},
			getList() {
				let id = uni.getStorageSync('customer_id');
				// 获取做题列表
				uni.request({
					url: config.url + '/app/qa/special/list?courseid=' + this.curryindex, //仅为示例，并非真实接口地址。
					data: {},
					success: (res) => {
						this.list = res.data.data;
					}
				});
			},
			goToRead() {
				uni.navigateTo({
					url: '../topic/topic?source='+this.curryindex
				});
			},
			goToHistory() {
				uni.navigateTo({
					url: '../history/history'
				});
			},
			goToReport() {
				uni.navigateTo({
					url: '../studyReport/studyReport'
				});
			}
		}
	}
</script>

<style scoped lang="scss">
	page {
		height: 100%;
		overflow-y: scroll;
	}

	.task-content {
		min-height: 100vh;
		width: 100%;
		background-color: #fff;
		overflow-y: scroll;
		.list {
			display: flex;
			margin-bottom: 20rpx;
			margin-top: 30rpx;

			.item {
				flex: 1;
				text-align: center;
				font-size: 24rpx;

				.i {
					display: block;
					font-size: 80rpx;
				}
			}
		}

		.practice-title {
			margin: 40rpx 0px 0rpx 30rpx;
			display: inline-block;
		}

		.list-info {
			margin: 20rpx 0px 0rpx 20rpx;
		}

		.collapse-info {
			margin-top: 40rpx;
		}
		.uni-collapse{
			margin-top: -26rpx;
		}
	}

	.example {
		padding: 0 30upx 30upx
	}

	.example-title {
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 32upx;
		color: #464e52;
		padding: 30upx;
		margin-top: 20upx;
		position: relative;
		background-color: #fdfdfd
	}

	.example-title__after {
		position: relative;
		color: #031e3c
	}

	.example-title:after {
		content: '';
		position: absolute;
		left: 0;
		margin: auto;
		top: 0;
		bottom: 0;
		width: 10upx;
		height: 40upx;
		border-top-right-radius: 10upx;
		border-bottom-right-radius: 10upx;
		background-color: #031e3c
	}

	.example .example-title {
		margin: 40upx 0
	}

	.example-body {
		padding: 30upx;
		background: #fff
	}

	.example-info {
		padding: 30upx;
		color: #3b4144;
		background: #fff
	}

	.content {
		padding: 30upx;
		background: #f9f9f9;
		color: #666;
	}
</style>
