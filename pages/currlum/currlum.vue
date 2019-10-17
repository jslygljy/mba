<template>
    <view class="currlum-content">
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name" :scroll="true"></sun-tab>
		<view class="list-item">
			<view class="list-item-content" v-for="(item,index) in list" :key="item.innerid" @click="goToDetail(item.innerid)">
				<view class="flex-sub">
					<image :src="item.speaker_heading" mode=""></image>
				</view>
				
				<view class="item-right flex-treble">
					<view style="flex:4">
						<text class="h4">{{item.title}}</text>
						<text class="grey">主讲:{{item.speaker_name}}</text>
					</view>
					<view class="item-bottom">
						<!-- <text class="grey">已学{{ item.status==item.status? '完' :item.status+'/'+item.status+'课时' }}</text> -->
						<text class="grey">已学{{item.speed}}</text>
					</view>
				</view>
			</view>
		</view>
    </view>
</template>

<script>
 
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import config from '../../config.js';
    export default {
		components:{
			sunTab
		},
		data() {
		    return {
				index: 0,
				
				list:[{
					'title':'真题课程包',
					'teacher':'123',
					'status':'30',
					'id':1,
					'coverimg':'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
				}],
                tabObjectList: [ //对象数组赋值
                    {
                        name: '全部',
                        value: ''
                    },
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
                    }
                ],
			}
		},
		onShow(){
			this.getList('');
		},
		methods:{
            objectChange(e){
                this.getList(e.tab.value)
            },
			goToDetail(id){
				uni.reLaunch({
				    url: '../curlumDetail/curlumDetail?course_id='+id
				});
			},
			getList(type){
				let userid =uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url+'/app/course/list/'+userid,
					// url: config.url+'/app/sgincourse/list/'+userid,
				    data: {
				        type: type
				    },
				    success: (res) => {
						if(res.data.errcode==0){
							this.list = res.data.data
						}else{
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
				width: 200rpx;
				height: 200rpx;
				border-radius: 10rpx;
			}
			.item-right{
				font-size: 24rpx;
				margin-left: 40rpx;
				display: flex;
				flex-direction: column;
				margin-right: 20rpx;
				.h4{
					font-size: 32rpx;
					margin-top: 2rpx;
					display: block;
					margin-bottom: 10rpx;
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
