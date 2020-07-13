<template>
    <view class="find-content">
		<view class="list flex">
			<!-- <view class="bg-gradual-purple padding radius text-center shadow-blur flex-sub margin-lr" style="height: 150rpx;">
				<view class="text-lg" style="line-height: 3;">书城</view>
			</view> -->
			<view class="bg-gradual-blue padding radius text-center shadow-blur flex-sub margin-lr" style="height: 150rpx;" @click="goToUc">
				<view class="text-lg" style="line-height: 3;">院校指南</view>
			</view>
		</view>
		
		<sun-tab class="tabs" :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name" :scroll="true" style="margin-top: 40upx;"></sun-tab>
		<mescroll-uni :down="downOption" @down="downCallback" :up="upOption" @up="upCallback" :fixed="false" bottom="20" @init="mescrollInit">
			<view class="list-item" v-for="(data,itemindex) in list" :key="itemindex" @click="goToDetail(data)">
				<view class="list-item-content">
					<view class="item-right">
						<view style="flex:4">
							<h4 class="h4">{{data.title}}</h4>
						</view>
						<view class="item-bottom">
							<text class="price">{{data.name}}</text>
							<text class="sale"></text>
						</view>
					</view>
					<image :src="data.cover" mode=""></image>
				</view>
			</view>
		</mescroll-uni>
    </view>
</template>

<script>
    import {
        mapState
    } from 'vuex'
	import bwSwiper from '@/wxcomponents/bw-swiper/bw-swiper.vue'
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import MescrollUni from "@/components/mescroll-uni/mescroll-uni.vue";
	import config from '../../config.js';
    export default {
        computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
		components:{
			bwSwiper,
			sunTab,
			MescrollUni
		},
		data() {
		    return {
				swiperList:[{img: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'},{img: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'},{img: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'}],
				index: 0,
				mescroll: null,
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
				list:[],
				pageindex:0,
                tabObjectList: [],
			}
		},
		onShow() {
			this.getType();
		},
		methods:{
			getType(){
				let id = uni.getStorageSync('customer_id');
				uni.request({
					url: config.url + '/app/read/type',
					method:"GET",
					data: {
					},
					success: (res) => {
						res.data.data.map((date,index)=>{
							this.tabObjectList.push({
							    name: date,
							    value: index
							})
						})
					}
				});
					
			},
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
					url: config.url + '/app/read/list/'+(this.index+1)+'?pageindex='+pageindex,
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
			arrayChange(e){
                console.log('数组数据返回格式');
                console.log(e);
            },
            objectChange(e){
				this.index = e.tab.value;
				this.upCallback(this.mescroll);
            },
			onScroll(e) {
			    console.log(e.$el.scrollTop)
			},
			goToRead(){
				uni.navigateTo({
				    url: '../read/read',
				});
			},
			goToUc(){
				uni.navigateTo({
				    url: '../ucList/ucList',
				});
			},
			onLoadMore(e) {
			    setTimeout(() => {
			        for (let i = 1; i <= 5; i++) {
			           console.log(2);
			        }
			    }, 1000 * 1)
			},
			onPullDown(e){
				console.log(3);
			},
			goToDetail(item){
				uni.navigateTo({
				    url: '/pages/readDetail/readDetail?id='+item.innerid,
				});
			},
		}
		
    }
</script>

<style scoped lang="scss">
	.find-content{
		width: 100%;
		flex: 1;
		background-color: #fff;
		height: 100%;
		display: flex;
		flex-direction: column;
		.uni-tab{
			line-height: 26px;
		}
	}
	.loadingText {
	    line-height: 30px;
	    text-align: center;
	    font-size: 12px;
	    color: #999;
	}
	.list-item{
		margin-top: 40rpx;
		font-size: 40rpx;
		border-bottom: 2rpx #C0C0C0 solid;
		padding-bottom: 40rpx;
		&:last-child{
			border: none;
		}
		.list-item-content{
			display: flex;
			margin-top: 30rpx;
			image{
				width: 320rpx;
				height: 180rpx;
				margin-left: 30rpx;
				margin-right: 10rpx;
			}
			.item-right{
				font-size: 24rpx;
				margin-left: 10rpx;
				display: flex;
				flex-direction: column;
				.h4{
					font-size: 30rpx;
					margin-top: 2rpx;
				}
				.grey{
					color: #8f8f94
				}
				.item-bottom{
					flex:1
				}
				.price{
					color: #8f8f94;
				}
				.sale{
					margin-left: 20rpx;
					color: #8f8f94;
				}
				.buy{
					color: #8f8f94;
					margin-left: 60rpx;
				}
			}
		}
	}
	.list{
		display: flex;
		margin-bottom: 20rpx;
		margin-top: 20rpx;
		.item{
			flex:1;
			text-align: center;
			font-size: 24rpx;
		}
		.i{
			margin-left: 60rpx;
			margin-top: 20rpx;
			margin-bottom: 20rpx;
		}
	}
	.ad{
		
		width: 100%;
		height: 100rpx;
		margin-bottom: 20rpx;
		image{
			margin: 0px 0rpx 0px 10rpx;
			width: 710rpx;
			height: 100%;
		}
	}
    .title {
        color: #8f8f94;
        margin-top: 50upx;
    }
	.icon1{
		background-image: url('/static/img/bookmark.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}
	.icon2{
		background-image: url('/static/img/notebook.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}
	.icon3{
		background-image: url('/static/img/report.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}
	.icon4{
		background-image: url('/static/img/monitor.png');
		display: block;
		width: 64upx;
		height: 64upx;
		background-size: 100% 100%;
	}

</style>
