<template>
    <view class="curlum-detail">
        <video id="myVideo" src="https://dcloud-img.oss-cn-hangzhou.aliyuncs.com/guide/uniapp/%E7%AC%AC1%E8%AE%B2%EF%BC%88uni-app%E4%BA%A7%E5%93%81%E4%BB%8B%E7%BB%8D%EF%BC%89-%20DCloud%E5%AE%98%E6%96%B9%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B@20181126.mp4"
                    @error="videoErrorCallback" controls></video>
		
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name"></sun-tab>
		<!-- 简介 -->
		<view class="info" v-if="index==0">
			<text class="title">{{curlumName}}</text>
			<text class="subtitle text-darkGrey">{{curlumSubInfo}}</text>
			<view class="text-darkGrey info-type flex justify-between">
				<text class="">授课方式：{{curlumType}} | {{curlumNum}}课时</text>
				<view class="">
					<text class="cuIcon-attentionfavorfill text-darkGrey text-lg"></text>
					<text class="margin-left-sm">312</text>
				</view>
				
			</view>
			<text class="price">
				{{price}}
			</text>
		</view>
		<!-- 目录 -->
		<view class="directory" v-if="index==1">
			<uni-collapse @change="change">
				<uni-collapse-item title="助力20备考" :show-animation="true">
					<view class="item flex" v-for="(item,subindex) in directoryList" :key="item.id" @click="setPlay(subindex)">
						<view class="item-left">
							<text :class="[ item.isPlaying?'text-blue':'text-darkGrey','cuIcon-videofill','text-xxxl']"></text>
						</view>
						<view :class="[ item.isPlaying?'text-blue':'', 'item-right','text-sm']" style="flex:8">
							<text class="text-df margin-top-xs block">{{item.title}}</text>
							<view class="item-time">
								<text>{{item.allTime}}</text>
								<text :class="[item.status ==0?'':'text-blue','margin-left-lg']">{{item.status ==0?'已学完':'上次观看至'+item.lastLearnTime}}</text>
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
		<view class="comment-post">
			<input type="text" :value="commentInfo" placeholder="请输入你的评论"/>
			<text class="post-button">发布</text>
		</view>
    </view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import comment from '@/components/commoent/uni-comment.vue';
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item-curlum/uni-collapse-item-curlum.vue'
    export default {
		components:{
			sunTab,
			comment,
			uniCollapse,
			uniCollapseItem
		},
        data() {
            return {
				curlumName:'助力20备考',
				curlumSubInfo:'导学指南 制胜攻略',
				price:'免费',
				curlumType:'点播',
				curlumNum:'1',
				commentInfo:'',
				index: 0,
				PlayNum:0,
				directoryList:[{
					id:1,
					videoSrc:'',
					title:'助力20备考逻辑上',
					allTime:'32:55',
					status:'0',  //0是已学完 1未学完
					lastLearnTime:'18：29',
					isPlaying:false
				},{
					id:2,
					videoSrc:'',
					title:'助力20备考逻辑下',
					allTime:'32:55',
					status:'1',
					lastLearnTime:'18：29',
					isPlaying:true
				}],
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
            }
        },
        methods: {
            videoErrorCallback: function(e) {
				uni.showModal({
					content: e.target.errMsg,
					showCancel: false
				})
			},
			objectChange(e){
			    console.log('对象数据返回格式');
			    console.log(e);
				this.index = e.tab.value;
			},
			change(e) {
				console.log(e)
			},
			setPlay(index){
				let c = [];
				this.directoryList.forEach((data)=>{
					data.isPlaying = false;
					c.push(data)
				});
				c[index].isPlaying = true;
				this.directoryList = c;
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
		.item{
			width: 100%;
			.item-left{
				margin: 60rpx 30rpx 0rpx;
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
	}
}
</style>
