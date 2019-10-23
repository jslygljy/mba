<template>
    <view class="read">
       <h4 class="lastnew">最新</h4>
	   <view class="bg-img bg-mask text-white" style="background-image: url('https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg');">
			<view class="info">
				<span class="icon"></span>
				独立包2019-09-03
			</view>
			<view class="right-tag">
				<span class="icon1"></span>
				已学习
			</view>
			<view class="padding-xs text-lg title">
				科学家减肥法打算导致南极洲变绿
			</view>
	   </view>
	   <h4 class="lastnew">往期</h4>
		<you-scroll ref="scroll" @onPullDown="onPullDown" @="" @onLoadMore="onLoadMore">
            <view class="cu-timeline"  v-for="(swiper,index) in list" :key="index">
				<view class="cu-item" @click="goToDetail">
					<view class="list-content">
						<view class="flex-twice margin-right">
							<text>{{swiper.name}}</text>
							<text class="time">{{swiper.value}}</text>
						</view>
						<view class="flex-sub">
							<image class="flex-sub" :src="swiper.src" mode=""></image>
						</view>
						
					</view>
				</view>
             </view>
            <div class="loadingText">正在努力加载...</div>
        </you-scroll>
		  
    </view>
</template>

<script>
	import youScroll from '@/components/you-scroll'
    export default {
		components: {
			youScroll
		},
        onLoad() {
        },
		onReady() {
            //console.log(this.$refs)
        },
		data() {
			return {
				list: [
					{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},
					{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					},{
						name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
						value: '维保 2019-09-22',
						src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
					}
				],
			}
		},
		methods: {
			
            onScroll(e) {
                console.log(e.$el.scrollTop)
            },
			goToDetail(){
				uni.navigateTo({
				    url: '/pages/readDetail/readDetail',
				});
			},
            onLoadMore(e) {
                setTimeout(() => {
                    for (let i = 1; i <= 5; i++) {
                        this.list.push({
							name: '研究法下降是大师级大师的撒的撒第三季度卡死了打',
							value: '维保 2019-09-22',
							src:'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
						})
                    }
                }, 1000 * 1)
            }
		}
    }
</script>

<style scoped lang="scss">
    @import "../../static/icon.css";
    @import "../../static/main.css";
	page {
        height: 100%;
    }
	.block {
            height: 200px;
            line-height: 200px;
            text-align: center;
            margin: 10px;
            background-color: aquamarine;
        }

        .loadingText {
            line-height: 30px;
            text-align: center;
            font-size: 12px;
            color: #999;
        }
	.read{
		background-color: #fff;
		width: 100%;
	}
	.lastnew{
		margin: 20rpx 0px 10px 2.5%;
	}
	.bg-img{
		font-size: 24rpx;
		width: 95%;
		margin-left: 2.5%;
		border-radius: 10rpx;
		height: 414upx;
		margin-bottom: 10rpx;
		.icon1{
			background-image: url('/static/img/wallet.png');
			background-size: 100% 100%;
			width: 28upx;
			height: 28upx;
			display: inline-block;
			vertical-align: sub;
			margin-right: 4rpx;
		}
		.info{
			margin-top: 25rpx;
			display: inline-block;
			vertical-align: sub;
			margin-left: 15rpx;
			.icon{
				background-image: url('https://iconfont.alicdn.com/t/1550728574104.jpg@200h_200w.jpg');
				background-size: 100% 100%;
				width: 40upx;
				height: 40upx;
				border-radius: 50%;
				display: inline-block;
				vertical-align: sub;
				margin-right: 15rpx;
			}
		}
		.right-tag{
			float:right;
			padding: 4px 8rpx;
			border-radius: 10rpx;
			border: 1px #fff solid;
			margin: 18rpx 18rpx 0px 0px;
		}
		.title{
			margin-top: 270rpx;
			margin-left: 6rpx;
		}
	}
	.cu-timeline{
		.cu-item{
			.list-content{
				display: flex;
				font-size: 24rpx;
				color: #000;
				image{
					width: 220rpx;
					height: 120rpx;
				}
				.time{
					display: block;
					margin-top: 20rpx;
					color: #ccc;
				}
			}
		}
	}
</style>
