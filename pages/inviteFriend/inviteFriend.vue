<template>
    <view class="inviteContent">
		<swiper class="card-swiper" :class="dotStyle?'square-dot':'round-dot'" :indicator-dots="true" :circular="true"
		 :autoplay="true" interval="5000" duration="500" @change="cardSwiper" indicator-color="#8799a3"
		 indicator-active-color="#0081ff">
			<swiper-item v-for="(item,index) in swiperList" :key="index" :class="cardCur==index?'cur':''">
				<view class="swiper-item">
					<image :src="item.url" mode="aspectFill" v-if="item.type=='image'"></image>
					<video :src="item.url" autoplay loop muted :show-play-btn="false" :controls="false" objectFit="cover" v-if="item.type=='video'"></video>
				</view>
			</swiper-item>
		</swiper>
		<view class="">
			<text>好友注册你将获得40元学习礼包</text>
			<text>好友也将领取同等价值学习礼包</text>
		</view>
		<view class="cu-list grid no-border">
			<view class="cu-item flex-sub" v-for="(item,index) in cuIconList" :key="index">
				<view :class="['cuIcon-' + item.cuIcon,'text-' + item.color]">
					<view class="cu-tag badge" v-if="item.badge!=0">
						<block v-if="item.badge!=1">{{item.badge>99?'99+':item.badge}}</block>
					</view>
				</view>
				<text>{{item.name}}</text>
			</view>
		</view>
    </view>
</template>

<script>
    
    export default {
    	components:{
    	},
        data() {
        	return {
        		cardCur: 0,
        		swiperList: [{
        			id: 0,
        			type: 'image',
        			url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big84000.jpg'
        		}, {
        			id: 1,
        			type: 'image',
        			url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big37006.jpg',
        		}, {
        			id: 2,
        			type: 'image',
        			url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big39000.jpg'
        		}, {
        			id: 3,
        			type: 'image',
        			url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big10001.jpg'
        		}, {
        			id: 4,
        			type: 'image',
        			url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big25011.jpg'
        		}, {
        			id: 5,
        			type: 'image',
        			url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big21016.jpg'
        		}, {
        			id: 6,
        			type: 'image',
        			url: 'https://ossweb-img.qq.com/images/lol/web201310/skin/big99008.jpg'
        		}],
        		dotStyle: false,
        		towerStart: 0,
        		direction: '',
				cuIconList: [{
					cuIcon: 'weixin',
					color: 'olive',
					badge: 0,
					name: '微信'
				}, {
					cuIcon: 'similar',
					color: 'orange',
					badge: 0,
					name: '朋友圈'
				}, {
					cuIcon: 'newshot',
					color: 'blue',
					badge: 0,
					name: '保存图片'
				}],
        	};
        },
        methods: {
            // cardSwiper
            cardSwiper(e) {
            	this.cardCur = e.detail.current
            },
        }
    }
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
    .card-swiper{
		height: 910upx !important
	}
</style>
