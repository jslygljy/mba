<template>
    <view>
        <view class="uni-padding-wrap">
            <view class="uni-comment">
                <view class="uni-comment-list" v-for="(item,index) in list" :key="index">
                    <view class="uni-comment-face">
                        <image :src="item.headImg || '/static/logo.png'" mode="widthFix"></image>
                    </view>
                    <view class="uni-comment-body">
                        <view class="uni-comment-top">
                            <text>{{item.nickname || '考研同学'}}</text>
                        </view>
                        <view class="uni-comment-content">{{item.content}}</view>
						<view class="uni-comment-reply" v-if="item.reply">
							<text class="text-blue">教学助教回复</text>:
							<text class="reply-content">{{item.reply}}</text>
						</view>
                        <view class="uni-comment-date">
                            <text style="color: #888;">{{item.createdtime}}</text>
							<view class="uni-comment-replay-btn">
								<text class="margin-right-xs">{{item.praise_count}}</text>
								<text v-if="item.is_praise" class="cuIcon-appreciatefill text-blue"></text>
								<text v-else class="cuIcon-appreciatefill text-grey" @click="onClick(item,index)"></text>
							</view>
                        </view>
                    </view>
                </view>
                
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                title: "评论界面"
            }
        },
		mounted(){
		},
		props: {
		    list: {
				type: Array,
				default () {
					return [];
				}
			}
		},
		methods: {
			onClick (item,index) {
				console.log(item.newType);
				if(item.newType=="hot"){
					this.$emit('thumbsGoodUp',index)
				}else{
					console.log(this.list)
					this.$emit('thumbsListUp',{
						index,
						item
					})
				}
			  
			}
		}
    }
</script>

<style scoped lang="scss">
    /* comment */
    .uni-padding-wrap {
        padding:0rpx 30upx;
    }
    .uni-comment {
        padding: 5rpx 0;
        display: flex;
        flex-grow: 1;
        flex-direction: column;
		height: 100%
    }

    .uni-comment-list {
        flex-wrap: nowrap;
        padding: 20rpx 0 20rpx;
        width: 100%;
        display: flex;
		flex:1;
		border-bottom: 1rpx #ddd solid;
    }

    .uni-comment-face {
        width: 70upx;
        height: 70upx;
        border-radius: 100%;
        margin-right: 20upx;
        flex-shrink: 0;
        overflow: hidden;
		margin-top: 20rpx;
    }

    .uni-comment-face image {
        width: 100%;
        border-radius: 100%;
    }

    .uni-comment-body {
        width: 100%;
    }

    .uni-comment-top {
        line-height: 1.5em;
        justify-content: space-between;
		margin-top: 2rpx;
    }

    .uni-comment-top text {
        font-size: 24upx;
		color: #3e6d94;
    }

    .uni-comment-date {
        line-height: 38upx;
        flex-direction: row;
        justify-content: space-between;
        display: flex !important;
        flex-grow: 1;
		font-size: 24rpx;
		padding-bottom: 15rpx;
    }

    .uni-comment-date view {
        color: #666666;
        font-size: 24upx;
        line-height: 38upx;
    }

    .uni-comment-content {
        line-height: 1.6em;
        font-size: 28upx;
        padding: 8rpx 0 25rpx;
    }
	.uni-comment-reply{
		background-color: #ddd;
		font-size: 24rpx;
		border-radius: 10rpx;
		padding: 5rpx 10rpx;
		margin-bottom: 24rpx;
		.reply-content{
			font-size: 24rpx;
		}
	}
</style>
