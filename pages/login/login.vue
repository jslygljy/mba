<template>
    <view class="login-content">
        <view class="input-group">
            <view class="border">
                <text class="title">手机号码</text>
                <input class="m-input" type="number" clearable focus v-model="account" placeholder="请输入手机号码" style="font-size: 26px;"></input>
            </view>
        </view>
        <view class="btn-row">
            <button type="primary" :class="account.length!==11? 'grey':'primary'" @click="bindLogin">获取验证码</button>
        </view>
        <view class="action-row">
            <navigator url="../main/main" open-type="switchTab" >试用一下</navigator>
        </view>
        <!-- <view class="oauth-row" v-if="hasProvider" v-bind:style="{top: positionTop + 'px'}">
            <view class="oauth-image" v-for="provider in providerList" :key="provider.value">
                <image :src="provider.image" @tap="oauth(provider.value)"></image>
            </view>
        </view> -->
		<view class="bottom-row">
			<!-- 登录即代码同意<navigator url="../reg/reg" class="goToHref">《MBA大师用户协议》</navigator> -->
		</view>
    </view>
</template>

<script>
    import service from '../../service.js';
	import config from '../../config.js';
    import {
        mapState,
        mapMutations
    } from 'vuex'
    import mInput from '../../components/m-input.vue'

    export default {
        components: {
            mInput
        },
        data() {
            return {
                providerList: [],
                hasProvider: false,
                account: '',
                password: '',
                positionTop: 0
            }
        },
        computed: mapState(['forcedLogin']),
        methods: {
            ...mapMutations(['login']),
            
            bindLogin() {
                if (this.account.length !== 11) {
                    uni.showToast({
                        icon: 'none',
                        title: '手机号为11个字符'
                    });
                    return;
                };
				uni.request({
				    url: config.url+'/app/sms/send/'+this.account, //仅为示例，并非真实接口地址。
				    data: {
				        type: '1',
					},
				    success: (res) => {
				        if(res.errcode==0){
							uni.showToast({
								title: '验证码发送成功'
							});
						}
				    }
				});
				uni.navigateTo({
				    url: '../loginCode/loginCode?tel='+this.account,
				});
               
            },
            toMain(userName) {
                this.login(userName);
                /**
                 * 强制登录时使用reLaunch方式跳转过来
                 * 返回首页也使用reLaunch方式
                 */
                if (this.forcedLogin) {
                    uni.switchTab({
                        url: '../main/main',
                    });
                } else {
                    uni.navigateBack();
                }

            }
        },
        onReady() {
        }
    }
</script>

<style scoped lang="scss">
	.login-content{
		width: 100%;
		background-color: #fff;
		.input-group{
			margin: 40rpx 0px 0px 0px;
		}
		.m-input{
			margin-top: 15rpx;
			width: 100%;
			border-bottom: 2rpx #ccc solid;
			height: 100rpx;
			line-height: 100rpx;
			.uni-input-placeholder{
				font-size: 26px;
			}
		}
		.btn-row{
			margin-top: 150rpx;
		}
		.primary{
			background-color: #007aff;
			width: 640rpx;
			height: 80rpx;
			line-height: 80rpx;
			border-radius: 30rpx;
			color: #fff;
		}
		.grey{
			background-color: gainsboro;
			width: 640rpx;
			height: 80rpx;
			line-height: 80rpx;
			border-radius: 30rpx;
			color: #fff;
		}
		.action-row{
			color: #ccc;
			font-size: 24rpx;
			text-align: center;
			width: 100%;
			margin-top: 30rpx;
		}
		.bottom-row{
			position: fixed;
			bottom: 40rpx;
			font-size: 24rpx;
			text-align: center;
			width: 100%;
			.goToHref{
				color: dodgerblue;
				display: inline-block;
			}
		}
		.input-group {
			background-color: #ffffff;
			margin: 40upx;
			position: relative;
		}
		.input-group::after {
			position: absolute;
			right: 0;
			bottom: 0;
			left: 0;
			height: 1upx;
			content: '';
			-webkit-transform: scaleY(.5);
			transform: scaleY(.5);
			background-color: #c8c7cc;
		}
		
	}
    
</style>
