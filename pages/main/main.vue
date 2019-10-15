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
			<image src="../../static/main/activity1.jpg" mode=""></image>
		</view>
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name" :scroll="true"></sun-tab>
		
		<!--  推荐课程  -->
		<h3 class="list-title">{{tuijianlist.title}}</h3>
		<view class="list-item" v-for="(item,index) in tuijianlist.list" :key="index.id">
			<view class="list-item-content">
				<image :src="item.coverimg" mode=""></image>
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
		
		<!--  热门课程  -->
		<h3 class="list-title">{{hotlist.title}}</h3>
		<view class="list-item" v-for="(item,index) in hotlist.list" :key="index.id">
			<view class="list-item-content">
				<image :src="item.coverimg" mode="" class="people"></image>
				<view class="item-right">
					<view style="flex:5">
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
		
		<!--  真题课程包  -->
		<h3 class="list-title">{{zhentilist.title}}</h3>
		<view class="list-item" v-for="(item,index) in zhentilist.list" :key="index.id">
			<view class="list-item-content">
				<image :src="item.coverimg" mode=""></image>
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
		
		<!--  每日一练  -->
		<h3 class="list-title">{{meirilist.title}}</h3>
		<view class="list-item" v-for="(item,index) in meirilist.list" :key="index.id">
			<view class="list-item-content">
				<image :src="item.coverimg" mode=""></image>
				<view class="item-right">
					<view style="flex:4">
						<h4 class="h4">{{item.name}}</h4>
						<text class="grey">{{item.subinfo}}</text>
					</view>
					<!-- <view class="item-bottom">
						<text class="price">{{item.price}}</text>
						<text class="sale">{{item.sale}}</text>
						<text class="buy">{{item.buy}}人已购</text>
					</view> -->
				</view>
			</view>
		</view>
		
		<!--  免费系统课程  -->
		<h3 class="list-title">{{mianfeilist.title}}</h3>
		<view class="list-item" v-for="(item,index) in mianfeilist.list" :key="index.id">
			<view class="list-item-content">
				<image :src="item.coverimg" mode="" class="people"></image>
				<view class="item-right">
					<view style="flex:5">
						<h4 class="h4">{{item.name}}</h4>
						<text class="grey">{{item.subinfo}}</text>
					</view>
					<view class="item-bottom">
						<text class="price">免费</text>
						<text class="buy">{{item.buy}}人已报名</text>
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
				swiperList:[{img: '../../static/main/banner1.png'},{img: '../../static/main/banner2.png'},{img: '../../static/main/banner3.png'}],
				index: 0,
				tuijianlist:{
					'title':'推荐课堂',
					list:[{
						'name':'0基础抱佛脚课     直播精选+真题视频',
						'subinfo':'0基础带你过线，实现逆袭',
						'price':'$12999',
						'sale':'$2998',
						'buy':'4772',
						'id':1,
						'coverimg':'../../static/main/activity1.jpg'
					},{
						'name':'0基础上岸包增加写作/押题/冲刺等',
						'subinfo':'打包购买省1100',
						'price':'$15998',
						'sale':'$3298',
						'buy':'4598',
						'id':2,
						'coverimg':'../../static/main/activity2.png'
					},{
						'name':'绝密押题课',
						'subinfo':'押题业界NO.1,考前神助攻',
						'price':'$2199',
						'sale':'$299',
						'buy':'3830',
						'id':3,
						'coverimg':'../../static/main/activity3.png'
					}]
				},
				zhentilist:{
					'title':'真题课程包',
					list:[{
						'name':'真题课程包',
						'subinfo':'提分神器 上岸必刷',
						'price':'$3180',
						'sale':'$798',
						'buy':'2.4万',
						'id':9,
						'coverimg':'../../static/main/zhenti.png'
					}]
				},
				mianfeilist:{
					'title':'免费系统课程',
					list:[{
						'name':'全新逻辑系统课-形式逻辑',
						'subinfo':'告别晦涩 一听即懂',
						'buy':'35.3万',
						'id':10,
						'coverimg':'../../static/main/people1.png'
					},{
						'name':'全新逻辑系统课-论证推理',
						'subinfo':'提炼本质 精准解题',
						'buy':'35万',
						'id':11,
						'coverimg':'../../static/main/people2.png'
					},{
						'name':'2020年数学系统课MBA、MPA、MEM、MTA',
						'subinfo':'从零学起 稳固基础',
						'buy':'35.5万',
						'id':12,
						'coverimg':'../../static/main/people3.png'
					},{
						'name':'2020年数学系统课MPACC、MAUD、MLIS',
						'subinfo':'掌握秒杀 精准得分',
						'buy':'7.9万',
						'id':13,
						'coverimg':'../../static/main/people4.png'
					},{
						'name':' 2020年语法与长难句精选',
						'subinfo':'剖析句型 易学易懂',
						'buy':'12.2万',
						'id':14,
						'coverimg':'../../static/main/people5.png'
					}]
				},
				meirilist:{
					'title':'每日一练',
					list:[{
						'name':'每日阅读【每天一篇外文期刊文章】',
						'subinfo':'更新至2019-10-15期',
						'id':10,
						'coverimg':'../../static/main/ri1.png'
					},{
						'name':'英语一恋【每周二/四/六与公众号同步更新】',
						'subinfo':'更新至2019-10-15期',
						'id':11,
						'coverimg':'../../static/main/ri2.png'
					}]
				},
				hotlist:{
					'title':'热门课堂',
					list:[{
						'name':'0基础抱佛脚课-逻辑&写作',
						'subinfo':'直击考点 快速提分',
						'price':'$4333',
						'sale':'$1298',
						'buy':'4784',
						'id':4,
						'coverimg':'../../static/main/people1.png'
					},{
						'name':'0基础抱佛脚课-数学',
						'subinfo':'告别题海 秒杀拿分',
						'price':'$4333',
						'sale':'$1298',
						'buy':'4784',
						'id':5,
						'coverimg':'../../static/main/people2.png'
					},{
						'name':'0基础抱佛脚课-英语',
						'subinfo':'技巧浓缩 高效得分',
						'price':'$4333',
						'sale':'$1298',
						'buy':'4785',
						'id':6,
						'coverimg':'../../static/main/people3.png'
					},{
						'name':'写作代写团',
						'subinfo':'八小时掌握写作高分技巧',
						'price':'$1198',
						'sale':'$398',
						'buy':'5664',
						'id':7,
						'coverimg':'../../static/main/people4.png'
					},{
						'name':'21天搞定800+核心词',
						'subinfo':'高频学习 立足得分',
						'price':'$698',
						'sale':'$198',
						'buy':'6184',
						'id':8,
						'coverimg':'../../static/main/people5.png'
					}]
				},
                tabObjectList: [ //对象数组赋值
                    {
                        name: '精品推荐',
                        value: 1
                    },
                    {
                        name: '真题课程',
                        value: 2
                    },
                    {
                        name: '逻辑课程',
                        value: 3
                    },
                    {
                        name: '数学课程',
                        value: 4
                    },
                    {
                        name: '英语课程',
                        value: 5
                    },
                    {
                        name: '写作课程',
                        value: 6
                    },
                    {
                        name: '提面及复试',
                        value: 7
                    },
                    {
                        name: '新手必看',
                        value: 8
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
	.list-title{
		margin-top: 50rpx;
		margin-left: 15rpx;
	}
	.list-item{
		margin-top: 50rpx;
		margin-left: 15rpx;
		font-size: 40rpx;
		.list-item-content{
			display: flex;
			margin-top: 30rpx;
			image{
				width: 280rpx;
				height: 190rpx;
				min-width: 280rpx;
			}
			.people{
				width: 190rpx;
				height: 190rpx;
				min-width: 190rpx;
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
