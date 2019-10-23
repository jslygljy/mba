<template>
    <view class="guide">
        <swiper class="flex1" interval="3000" :show-indicators="false" :auto-play="autoPlay" @change="sliderChange" :infinite="false" :forbid-slide-animation="false">
            <swiper-item class="flex1" v-for="(img, index) in imageList" :key="index">
                <view class="flex1">
                    <!-- #ifndef APP-PLUS -->
                    <image class="flex1" mode="aspectFill" :style="{ width: screenWidth+ 'px' }" :src="img.src" />

                    <!-- #endif -->

                    <!-- #ifdef APP-PLUS -->
                    <image class="flex1" resize="contain" :src="img.src" />
                    <!-- #endif -->
                </view>
            </swiper-item>
        </swiper>
		
        <view class="swiper-to-home" @click="launchApp">
			<view class="swiper-to-home-text">
				<min-countdown :targetTime="time1" @callback="launchApp" :format="format"></min-countdown>
			</view>
		</view>
    </view>
</template>

<script>
const SystemInfo = uni.getSystemInfoSync();
import service from '../../service.js';
import minCountdown from '@/components/min-countdown/min-countdown.vue'
export default {
	components: {
	    minCountdown
	  },
    data() {
        return {
            imageList: [
                {
                    src: '../../static/guide/guide_3.9.png'
                }
            ],
			time1: new Date().getTime() + 5000,
			format: `<div>
			        <span>{%s1}s跳过</span>
			        </div>`,
            autoPlay: false,
            currIndex: 0,
            screenWidth: SystemInfo.screenWidth
        };
    },
    methods: {
        sliderChange(e) {
            console.log(e);
            this.currIndex = e.detail.current;
        },
		callback(){
			
		},
        launchApp() {
            let id =uni.getStorageSync('customer_id');
            if (!id) {
                uni.navigateTo({
                    url: '../login/login'
                });
            } else {
                uni.switchTab({
                    url: '../main/main'
                });
            }
        }
    }
};
</script>
<style scoped>
.guide {
    /* #ifndef APP-PLUS */
    display: flex;
    height: 100%;
    /* #endif */
    flex-direction: column;
    flex: 1;
}

.flex1 {
    flex: 1;
	height: 100vh;
}
.apptestnnnn {
    border-width: 1px;
    border-color: #4cd964;
    border-style: solid;
}
.apptest {
    border-width: 1px;
    border-color: #007aff;
    border-style: solid;
}
.swiper-to-home {
    position: absolute;
    z-index: 999;
    right: 40rpx;
    top: 0rpx;
}

.swiper-to-home-text {
    color: #ffffff;
    text-align: center;
    background-color: rgba(0, 0, 0, 0.5);
    border-width: 1rpx;
    border-color: #ffffff;
    border-style: solid;
    border-radius: 30rpx;
    font-size: 28rpx;
    padding-top: 5rpx;
    padding-bottom: 5rpx;
    padding-left: 25rpx;
    padding-right: 25rpx;
}

.indicator {
    width: 714rpx;
    height: 30rpx;
    position: absolute;
    bottom: 50rpx;
    left: 1rpx;
    item-color: #f6f6f6;
    item-selected-color: #fd575c;
    item-size: 20rpx;
}
</style>
