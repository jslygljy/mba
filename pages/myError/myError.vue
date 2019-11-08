<template>
	<view class="error-content">
		<!-- <ChooseLits :list="filerList" :arr="arr" @chooseLike="chooseLike"></ChooseLits> -->
		<mescroll-uni :down="downOption" @down="downCallback" :up="upOption" @up="upCallback" :fixed="false" bottom="20" @init="mescrollInit">
			<uni-list class="list-info" v-if="list.length>0">
				<view class="flex solids-bottom lists" @click="onClick(item.innerid)" v-for="(item, index) in list" :key="item.innerid" v-if="list.length>0">
					<view class="uni-list-item__container">
					  <view class="uni-list-item__content">
						  <view class="uni-list-item__content-title text-df">{{ item.title }}</view>
					  </view>
					  <view class="flex justify-between align-center text-gray">
						<view class="uni-list-item__content-title text-sm">{{ item.ttitle }}</view>
						<text class="text-sm">
							{{(new Date(item.createdtime).getMonth()+1) + '-' + new Date(item.createdtime).getDate()}}
						</text>
					  </view>
					</view>
				</view>
			</uni-list>
		</mescroll-uni>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import config from '../../config.js';
	import ChooseLits from '@/components/choose-Cade/choose-Cade.vue'
	import MescrollUni from "@/components/mescroll-uni/mescroll-uni.vue";
	export default {
		components: {
			uniList,
			ChooseLits,
			MescrollUni
		},
		data() {
			return {
				list:[],
				mescroll: null,
				// filerList: ['综合排序', '类型不限', '金额不限'], 
				// arr: [ ['综合排序', '价格降序', '价格升序'], ['类型不限', '高通过率', '利率低'], ['金额不限', '5k以下', '5k-10k', '10k以上'] ],
				// tabObjectList: [ //对象数组赋值
				// 	{
				// 		name: '历年真题',
				// 		value: 0,
				// 	},
				// 	{
				// 		name: '模拟练习',
				// 		value: 1,
				// 	}
				// ],
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
			}
		},
		onLoad() {
		},
		onShow() {
			// this.getList();
		},
		methods: {
			mescrollInit(mescroll) {
				this.mescroll = mescroll;
			},
			downCallback(mescroll){
				mescroll.endSuccess()
			},
			getList(pageindex,cb) {
				let id = uni.getStorageSync('customer_id');
				// 推荐课程
				uni.request({
					url: config.url + '/app/qa/error/list/'+id+'?pageindex='+pageindex,
					method:"GET",
					data: {
					},
					success: (res) => {
						cb && cb(res.data.data)
					}
				});
			},
			upCallback(mescroll) {
				this.getListDataFromNet(mescroll.num, (curPageData) => {
					mescroll.endSuccess(curPageData.length);
					if (mescroll.num == 1) this.list = []; //如果是第一页需手动制空列表
					this.list = this.list.concat(curPageData); //追加新数据
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
			chooseLike(e){
				console.log(e)
			},
			onClick(qa_id){
				uni.navigateTo({
				    url: '../errDetail/errDetail?qa_id='+qa_id
				});
			}
			
		}

	}
</script>

<style lang="scss">


@mixin list-disabled {
	opacity: 0.3;
}
.error-content{
	width: 100%;
	height: 100%;
	background: #fff;
}
.paper-classify-item-name{
	background: rgba(32,102,255,.1);
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
		padding: $list-item-pd;
		width: 100%;
		box-sizing: border-box;
		position: relative;
		display: flex;
		flex-direction: column;
	}

	&__content {
		flex: 1;
		display: flex;
		color: #3b4144;
		&-title {
			color: inherit;
			line-height: 1.5;
			overflow: hidden;
			display: inline-block;
			padding-bottom: 10rpx;
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

