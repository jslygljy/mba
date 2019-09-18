<template>
    <view class="content">
        <bw-swiper :swiperList="swiperList" style="width:100%"></bw-swiper>
		<view class="list">
			<view class="item">
				<text class="icon1 i"></text>
				<text>书城</text>
			</view>
			<view class="item" @click="goToRead">
				<text class="icon2 i"></text>
				<text>每日阅读</text>
			</view>
			<view class="item" @click="goToCurriculum">
				<text class="icon3 i"></text>
				<text>我的课程</text>
			</view>
			<view class="item">
				<text class="icon4 i"></text>
				<text>学习报告</text>
			</view>
		</view>
		<view class="ad">
			<image src="https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg" mode=""></image>
		</view>
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name" :scroll="true"></sun-tab>
		
		<view class="list-item" v-for="(item,index) in list" :key="index">
			<h3>{{item.title}}</h3>
			<view class="list-item-content">
				<image src="https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg" mode=""></image>
				<view class="item-right">
					<view style="flex:4">
						<h4 class="h4">{{item.name}}</h4>
						<text class="grey">{{item.subinfo}}</text>
					</view>
					<view class="item-bottom">
						<text class="price">{{item.price}}</text>
						<text class="sale">{{item.sale}}</text>
						<text class="buy">{{item.buy}}人已购</text>
					</view>
				</view>
			</view>
		</view>
    </view>
</template>

<script>
    import {
        mapState
    } from 'vuex'
	import bwSwiper from '@/wxcomponents/bw-swiper/bw-swiper.vue'
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	
    export default {
        computed: mapState(['forcedLogin', 'hasLogin', 'userName']),
		components:{
			bwSwiper,
			sunTab
		},
		data() {
		    return {
				swiperList:[{img: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'},{img: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'},{img: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'}],
				index: 0,
				list:[{
					'title':'真题课程包',
					'name':'真题课程包',
					'subinfo':'提分神奇 商家看过',
					'price':'$777',
					'sale':'$7774',
					'buy':'33万',
					'id':1,
					'coverimg':'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
				},{
					'title':'真题课程包',
					'name':'真题课程包',
					'subinfo':'提分神奇 商家看过',
					'price':'$777',
					'sale':'$7774',
					'buy':'33万',
					'id':2,
					'coverimg':'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
				},],
                tabObjectList: [ //对象数组赋值
                    {
                        name: '选项卡一',
                        value: 1
                    },
                    {
                        name: '选项卡二',
                        value: 2
                    },
                    {
                        name: '选项卡三',
                        value: 3
                    },
                    {
                        name: '选项卡三',
                        value: 4
                    },
                    {
                        name: '选项卡三',
                        value: 5
                    }
                ],
			}
		},
        onLoad() {
			// 登录逻辑
       //      if (!this.hasLogin) {
       //          uni.showModal({
       //              title: '未登录',
       //              content: '您未登录，需要登录后才能继续',
       //              /**
       //               * 如果需要强制登录，不显示取消按钮
       //               */
       //              showCancel: !this.forcedLogin,
       //              success: (res) => {
       //                  if (res.confirm) {
							// /**
							//  * 如果需要强制登录，使用reLaunch方式
							//  */
       //                      if (this.forcedLogin) {
       //                          uni.reLaunch({
       //                              url: '../login/login'
       //                          });
       //                      } else {
       //                          uni.navigateTo({
       //                              url: '../login/login'
       //                          });
       //                      }
       //                  }
       //              }
       //          });
       //      }
        },
		methods:{
			arrayChange(e){
                console.log('数组数据返回格式');
                console.log(e);
            },
            objectChange(e){
                console.log('对象数据返回格式');
                console.log(e);
            },
			goToRead(){
				 uni.reLaunch({
				    url: '../read/read',
				});
			},
			goToCurriculum(){
				uni.reLaunch({
				    url: '../currlum/currlum',
				});
			}
		}
		
    }
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
	.content{
		padding: 10upx;
	}
	.list-item{
		margin-top: 50rpx;
		margin-left: 15rpx;
		font-size: 40rpx;
		.list-item-content{
			display: flex;
			margin-top: 30rpx;
			image{
				width: 320rpx;
				height: 200rpx;
			}
			.item-right{
				font-size: 24rpx;
				margin-left: 40rpx;
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
					color: #dc143c;
					font-size: 30rpx;
				}
				.sale{
					margin-left: 20rpx;
					color: #8f8f94;
					text-decoration:line-through;
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
