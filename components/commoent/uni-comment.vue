<template>
    <view>
        <view class="uni-padding-wrap">
            <view class="uni-comment">
                <view class="uni-comment-list" v-for="(item,index) in list" :key="index">
                    <view class="uni-comment-face">
                        <image :src="item.headImg" mode="widthFix"></image>
                    </view>
                    <view class="uni-comment-body">
                        <view class="uni-comment-top">
                            <text>{{item.nickName}}</text>
                        </view>
                        <view class="uni-comment-content">{{item.info}}</view>
                        <view class="uni-comment-date">
                            <text>{{item.time}}</text>
							<view class="uni-comment-replay-btn">
								{{item.likeNum}}
								<text v-if="item.isLike" class="cuIcon-appreciatefill text-blue"></text>
								<text v-else class="cuIcon-appreciatefill text-grey"></text>
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
		props: {
		  list: {
		    type: Array,
		    default () {
		    	return [];
		    }
			}
		},
    }
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
    /* comment */
    .uni-padding-wrap {
        padding: 30upx;
    }
    .uni-comment {
        padding: 5rpx 0;
        display: flex;
        flex-grow: 1;
        flex-direction: column;
    }

    .uni-comment-list {
        flex-wrap: nowrap;
        padding: 10rpx 0;
        margin: 10rpx 0;
        width: 100%;
        display: flex;
    }

    .uni-comment-face {
        width: 70upx;
        height: 70upx;
        border-radius: 100%;
        margin-right: 20upx;
        flex-shrink: 0;
        overflow: hidden;
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
    }

    .uni-comment-top text {
        font-size: 24upx;
    }

    .uni-comment-date {
        line-height: 38upx;
        flex-direction: row;
        justify-content: space-between;
        display: flex !important;
        flex-grow: 1;
		font-size: 24rpx;
    }

    .uni-comment-date view {
        color: #666666;
        font-size: 24upx;
        line-height: 38upx;
    }

    .uni-comment-content {
        line-height: 1.6em;
        font-size: 28upx;
        padding: 8rpx 0;
    }
</style>
