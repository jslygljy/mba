<template>
	<view class="topic-content">
		<uni-list class="list-info">
			<view class="flex solids-bottom lists" @click="goToStaillDetail(item.innerid)" v-for="item in list" :key="item.innerid">
				<view class="flex-four">
					<view class="uni-list-item__container">
						<view class="uni-list-item__content">
							<view class="uni-list-item__content-title">{{ item.title }}</view>
						</view>
					</view>
				</view>
				<view class="flex-sub">
					<text class="cuIcon-edit text-grey edit-button"></text>
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
	export default {
		components: {
			uniList
		},
		data() {
			return {
				title: '',
				list: [],
				source: 1,
				pages: 0,
				tabObjectList: [ //对象数组赋值
					{
						name: '历年真题',
						value: 0,
					},
					{
						name: '模拟练习',
						value: 1,
					}
				]
			}
		},
		onLoad(options) {
			this.source = options.source;
		},
		onShow() {
			this.getList();

		},
		methods: {
			getList() {
				let id = uni.getStorageSync('customer_id');
				// 推荐课程
				uni.request({
					url: config.url + '/app/topic/list?source=' + this.source,
					method: "GET",
					data: {},
					success: (res) => {
						this.list = res.data.data;
					}
				});
			},
			goToStaillDetail(id) {
				// let id =uni.getStorageSync('customer_id');
				if (this.source == 3) {
					// uni.navigateTo({
					// 	url: '../englishDetail/englishDetail?topicid=' + id + '&pages=' + (Number(this.pages)) +
					// 		'&showdetail=false&isTopic=true'
					// });
					uni.navigateTo({
						url: '../topic/english_template?topicid=' + id + '&pages=' + (Number(this.pages)) +
							'&showdetail=false&isTopic=true'
					});
				} else {
					// 获取做题列表
					uni.navigateTo({
						url: '../taskDetail/taskDetail?topicid=' + id + '&pages=' + (Number(this.pages)) +
							'&showdetail=false&isTopic=true'
					});
				}
			}
		}

	}
</script>

<style lang="scss">
	@mixin list-disabled {
		opacity: 0.3;
	}

	.topic-content {
		width: 100%;
		background: #fff;
	}

	.paper-classify-item-name {
		background: rgba(32, 102, 255, .1);
		border-radius: 8rpx;
		text-align: center;
		margin-bottom: 30rpx;
		margin-top: 60rpx;
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

	.progress {
		width: 70%;
		margin-left: 60rpx;
	}

	.lists {}

	.blueRound {
		display: inline-block;
		width: 15rpx;
		height: 15rpx;
		background-color: dodgerblue;
		vertical-align: top;
		border-radius: 50%;
		margin-top: 14rpx;
		margin-right: 20rpx;
	}

	.edit-button {
		position: absolute;
		right: 60rpx;
		top: 30rpx;
		font-size: 36rpx;
	}

	.uni-list-item {
		font-size: $uni-font-size-lg;
		position: relative;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		margin-top: 10rpx;
		padding-bottom: 20rpx;

		&--disabled {
			@include list-disabled;
		}

		&__container {
			padding: 30rpx;
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

	.uni-list>.uni-list-item:last-child .uni-list-item-container:after {
		height: 0px;
	}
</style>
