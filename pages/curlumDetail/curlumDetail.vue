<template>
    <view class="curlum-detail">
        <video id="myVideo" :src="vedio_url"
                    @error="videoErrorCallback" controls @timeupdate="videoTimeupdate"></video>
		
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
		</view>
		<!-- 目录 -->
		<view class="directory" v-if="index==1">
			<uni-collapse @change="change">
				<uni-collapse-item :title="curlumName" :show-animation="true">
					<view class="item flex" v-for="(item,subindex) in directoryList" :key="item.innerid" @click="setPlay(subindex,item.vedio_url,item.innerid)">
						<view class="item-left">
							<text :class="[ item.isPlaying?'text-blue':'text-darkGrey','cuIcon-videofill','text-xxxl']"></text>
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
			<text class="title">精彩评论({{goodlist.length}})</text>
			<comment :list="goodlist" @thumbsGoodUp="thumbsGoodUp"></comment>
			<text class="title">最新评论({{newlist.length}})</text>
			<comment :list="newlist" @thumbsListUp="thumbsListUp"></comment>
		</view>
		<view class="comment-post" v-if="is_sgin==1">
			<input type="text" :value="commentInfo" placeholder="请输入你的评论"/>
			<text class="post-button">发布</text>
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
    export default {
		components:{
			sunTab,
			comment,
			uniCollapse,
			uniCollapseItem
		},
        data() {
            return {
				curlumName:'',
				curlumSubInfo:'',
				old_price:'',
				curlumType:'',
				commentInfo:'',
				index: 0,
				PlayNum:0,
				vedio_url:'',
				buy_count:'',
				directoryList:[],
				goodlist:[{
					'headImg':'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
					'nickName':'今生缘',
					'info':'好牛逼的感觉，是不是小程序、App、移动端都互通了？',
					'time':'08/10 07:55',
					'likeNum':2,
					'isLike':true,
					'newType':'hot'
				},{
					'headImg':'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
					'nickName':'今生缘',
					'info':'好牛逼的感觉，是不是小程序、App、移动端都互通了？',
					'time':'08/10 07:55',
					'likeNum':2,
					'isLike':true,
					'newType':'hot',
					'reply':'是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的是的，是的'
				},{
					'headImg':'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
					'nickName':'今生缘',
					'info':'好牛逼的感觉，是不是小程序、App、移动端都互通了？',
					'time':'08/10 07:55',
					'likeNum':2,
					'isLike':false,
					'newType':'hot',
					'reply':'是的，是的'
				}],
				newlist:[{
					'headImg':'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
					'nickName':'今生缘',
					'info':'好牛逼的感觉，是不是小程序、App、移动端都互通了？',
					'time':'08/10 07:55',
					'likeNum':2,
					'isLike':false,
					'newType':'new',
					'reply':'是的，是的'
				},{
					'headImg':'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
					'nickName':'今生缘',
					'info':'好牛逼的感觉，是不是小程序、App、移动端都互通了？',
					'time':'08/10 07:55',
					'likeNum':2,
					'isLike':false,
					'newType':'new',
					'reply':'是的，是的'
				},{
					'headImg':'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
					'nickName':'今生缘',
					'info':'好牛逼的感觉，是不是小程序、App、移动端都互通了？2222',
					'time':'08/10 07:55',
					'likeNum':2,
					'isLike':false,
					'newType':'new',
					'reply':'是的，是的'
				}],
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
				course_id:'',
				is_sgin:'',
				book_id:'',
				videoContext:'',
				currentTime:''
            }
        },
		onShow(){
			this.getDetail();
			this.getMathDetail();
		},
		onReady: function(res) {
			// #ifndef MP-ALIPAY
			this.videoContext = uni.createVideoContext('myVideo');
			// #endif
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		   this.course_id = option.course_id;
		   this.is_sgin = option.is_sgin;
		},
		onUnload:function(){
			let userid =uni.getStorageSync('customer_id');
			uni.request({
				url: config.url+'/app/course/book/play/speed',
				method:"POST",
			    data: {
					'course_id':this.course_id,
					'customer_id':userid,
					'book_id':this.book_id,
					'speed_time':this.currentTime,
			    },
			    success: (res) => {
					if(res.data.errcode==0){
						
					}else{
						
					}
			        
			    }
			});
		},
        methods: {
			videoTimeupdate(e){
				this.currentTime = e.detail.currentTime;
			},
            videoErrorCallback: function(e) {
				// console.log(e);
				// uni.showModal({
				// 	content: e.target.errMsg,
				// 	showCancel: false
				// })
			},
			reportInfo(){
				let userid =uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url+'/app/sgincourse/sgin',
					method:"POST",
					data: {
					       "course_id":this.course_id,
						   "customer_id":userid
					},
				    success: (res) => {
						console.log(res)
						if(res.data.errcode==0){
							uni.showToast({
								title: '报名成功'
							});
							uni.reLaunch({
							    url: '../curlumDetail/curlumDetail?course_id='+this.course_id+'&is_sgin=1'
							});
						}else{
							uni.showToast({
								title: res.data.errmsg
							})
						}
				        
				    }
				});
				
			},
			objectChange(e){
			    
				this.index = e.tab.value;
			},
			change(e) {
				console.log(e)
			},
			setPlay(index,vedio_url,innerid){
				let c = [];
				this.directoryList.forEach((data)=>{
					data.isPlaying = false;
					c.push(data)
				});
				c[index].isPlaying = true;
				this.directoryList = c;
				if(vedio_url==''){
					uni.showToast({
						title: '暂无视频'
					});
					
				}else{
					this.vedio_url = vedio_url;
				}
				this.book_id = innerid;
			},
			thumbsListUp(index){
				this.newlist[index]['isLike'] = true;
				this.newlist[index]['likeNum'] = ++this.newlist[index]['likeNum'];
				uni.showToast({
					title: '点赞成功'
				});
			},
			thumbsGoodUp(index){
				this.goodlist[index]['isLike'] = true
				this.goodlist[index]['likeNum'] = ++this.goodlist[index]['likeNum'];
				uni.showToast({
					title: '点赞成功'
				});
			},
			// 获取课程详情
			getMathDetail(){
				let userid =uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url+'/app/course/detail/'+this.course_id+'/'+userid, //仅为示例，并非真实接口地址。
				    data: {
				    },
				    success: (res) => {
						console.log(res);
						if(res.data.errcode==0){
							this.curlumName = res.data.data.title;
							this.summary = res.data.data.summary.split('&&').join('');
							
							this.old_price = res.data.data.price
							this.buy_count = res.data.data.buy_count;
							this.curlumSubInfo = res.data.data.sub_tilte;
						}else{
							uni.showToast({
								title: res.data.errmsg
							})
						}
				        
				    }
				});
			},
			// 获取目录
			getDetail(){
				let userid =uni.getStorageSync('customer_id');
				// 全部
				uni.request({
					url: config.url+'/app/course/book/list/'+this.course_id+'/'+userid, //仅为示例，并非真实接口地址。
				    data: {
				    },
				    success: (res) => {
						if(res.data.errcode==0){
							if(res.data.data.length>0){
								this.directoryList = res.data.data;
								
								this.vedio_url = res.data.data[0].vedio_url;
								this.book_id = res.data.data[0].innerid;
							}else{
								uni.showToast({
									title: '暂无课程'
								})
							}
							
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
.curlum-detail{
	background-color: #fff;
	video{
		width: 100%
	}
	.info{
		border-bottom: 2rpx #ccc solid;
		font-size: 24rpx;
		.title{
			font-weight: bold;
			font-size: 34rpx;
			margin: 20rpx 30rpx;
			display: block;
		}
		.subtitle{
			font-size: 28rpx;
			margin: 20rpx 30rpx;
		}
		.info-type{
			font-size: 24rpx;
			margin: 20rpx 30rpx;
		}
		.price{
			color: red;
			font-size: 34rpx;
			display: block;
			margin: 20rpx 30rpx;
		}
	}
	.directory{
		height: 100%;
		padding-bottom: 100rpx;
		.item{
			width: 100%;
			.item-left{
				margin: 34rpx 30rpx 0rpx;
				width: 56rpx;
				height: 56rpx;
				max-width: 56rpx;
			}
			.item-right{
				border-bottom: 2rpx #ccc solid;
				padding-top: 30rpx;
				padding-bottom: 20rpx;
				.item-time{
					margin-top: 30rpx;
				}
			}
		}
	}
	.comment{
		height: 100%;
		display: inline-block;
		padding-bottom: 110rpx;
		.title{
			font-size: 32rpx;
			margin: 16rpx 0px 10rpx 26rpx;
			display: inline-block;
		}
		
	}
	.comment-post{
		position: fixed;
		bottom: 0px;
		height: 100rpx;
		display: flex;
		width: 100%;
		background: #fff;
		z-index: 1;
		input{
			flex:3;
			margin: 20rpx 0rpx 0px 30rpx;
			background-color: #eee;
			text-indent: 20rpx;
			font-size: 30rpx;
			height: 60rpx;
			color: #eef;
			border-radius: 10rpx;
		}
		.post-button{
			flex:1;
			margin: 20rpx 30rpx 0px 30rpx;
			border-radius: 40rpx;
			font-size: 28rpx;
			text-align: center;
			color: #fff;
			height: 60rpx;
			line-height: 60rpx;
			background-color: blue;
		}
		.post-button2{
			flex:1;
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
