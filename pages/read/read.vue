<template>
    <view class="read">
       <h4>最新</h4>
	   <view class="bg-img bg-mask flex align-center" style="background-image: url('https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg');height: 414upx;">
	   	<view class="padding-xl text-white">
	   		<view class="padding-xs text-xxl text-bold">
				已学习
	   		</view>
			<view class="">
				<span></span>
				独立包2019-09-03
			</view>
	   		<view class="padding-xs text-lg">
	   			科学家减肥法打算导致南极洲变绿
	   		</view>
	   	</view>
	   </view>
	   <view class="cu-timeline">
	   	<view class="cu-item">
	   		<view class="content">
	   			<view class="cu-capsule radius">
	   				<view class="cu-tag bg-cyan">上午</view>
	   				<view class="cu-tag line-cyan">10:00</view>
	   			</view>
	   			<view class="margin-top">这是第一次，我家的铲屎官走了这么久。久到足足有三天！！ 在听到他的脚步声响在楼梯间的那一刻，我简直想要破门而出，对着他狠狠地吼上10分钟，然后再看心情要不要他进门。</view>
	   		</view>
	   	</view>
	   </view>
	    <view class="cu-timeline">
	   	<view class="cu-item">
	   		<view class="content">
	   			<view class="cu-capsule radius">
	   				<view class="cu-tag bg-cyan">上午</view>
	   				<view class="cu-tag line-cyan">10:00</view>
	   			</view>
	   			<view class="margin-top">这是第一次，我家的铲屎官走了这么久。久到足足有三天！！ 在听到他的脚步声响在楼梯间的那一刻，我简直想要破门而出，对着他狠狠地吼上10分钟，然后再看心情要不要他进门。</view>
	   		</view>
	   	</view>
	   </view>
    </view>
</template>

<script>
    export default {
        onLoad() {
            if (!this.hasLogin) {
                uni.showModal({
                    title: '未登录',
                    content: '您未登录，需要登录后才能继续',
                    /**
                     * 如果需要强制登录，不显示取消按钮
                     */
                    showCancel: !this.forcedLogin,
                    success: (res) => {
                        if (res.confirm) {
							/**
							 * 如果需要强制登录，使用reLaunch方式
							 */
                            if (this.forcedLogin) {
                                uni.reLaunch({
                                    url: '../login/login'
                                });
                            } else {
                                uni.navigateTo({
                                    url: '../login/login'
                                });
                            }
                        }
                    }
                });
            }
        }
    }
</script>

<style scoped lang="scss">
    @import "./main.css";
    @import "./icon.css";
	.read{
		background-color: #fff;
	}
</style>
