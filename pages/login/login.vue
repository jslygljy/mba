<template>
    <view class="login-content">
        <view class="input-group">
            <view class="input-row border">
                <text class="title">手机号码</text>
                <input class="m-input" type="text" clearable focus v-model="account" placeholder="请输入手机号码" style="font-size: 26px;"></input>
            </view>
        </view>
        <view class="btn-row">
            <button type="primary" :class="account.length!==11? 'grey':'primary'" @tap="bindLogin">获取验证码</button>
        </view>
        <view class="action-row">
            <navigator url="../reg/reg">试用一下</navigator>
        </view>
        <!-- <view class="oauth-row" v-if="hasProvider" v-bind:style="{top: positionTop + 'px'}">
            <view class="oauth-image" v-for="provider in providerList" :key="provider.value">
                <image :src="provider.image" @tap="oauth(provider.value)"></image>
            </view>
        </view> -->
		<view class="bottom-row">
			登录即代码同意<navigator url="../reg/reg" class="goToHref">《MBA大师用户协议》</navigator>
		</view>
    </view>
</template>

<script>
    import service from '../../service.js';
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
            initProvider() {
                const filters = ['weixin', 'qq', 'sinaweibo'];
                uni.getProvider({
                    service: 'oauth',
                    success: (res) => {
                        if (res.provider && res.provider.length) {
                            for (let i = 0; i < res.provider.length; i++) {
                                if (~filters.indexOf(res.provider[i])) {
                                    this.providerList.push({
                                        value: res.provider[i],
                                        image: '../../static/img/' + res.provider[i] + '.png'
                                    });
                                }
                            }
                            this.hasProvider = true;
                        }
                    },
                    fail: (err) => {
                        console.error('获取服务供应商失败：' + JSON.stringify(err));
                    }
                });
            },
            initPosition() {
                /**
                 * 使用 absolute 定位，并且设置 bottom 值进行定位。软键盘弹出时，底部会因为窗口变化而被顶上来。
                 * 反向使用 top 进行定位，可以避免此问题。
                 */
                this.positionTop = uni.getSystemInfoSync().windowHeight - 100;
            },
            bindLogin() {
                
                if (this.account.length !== 11) {
                    uni.showToast({
                        icon: 'none',
                        title: '手机号为11个字符'
                    });
                    return;
                }
				uni.reLaunch({
				    url: '../loginCode/loginCode?tel='+this.account,
				});
                /**
                 * 下面简单模拟下服务端的处理
                 * 检测用户账号密码是否在已注册的用户列表中
                 * 实际开发中，使用 uni.request 将账号信息发送至服务端，客户端在回调函数中获取结果信息。
                 */
                // const data = {
                //     account: this.account,
                //     password: this.password
                // };
                // const validUser = service.getUsers().some(function (user) {
                //     return data.account === user.account && data.password === user.password;
                // });
                // if (validUser) {
                //     this.toMain(this.account);
                // } else {
                //     uni.showToast({
                //         icon: 'none',
                //         title: '用户账号或密码不正确',
                //     });
                // }
            },
            oauth(value) {
                uni.login({
                    provider: value,
                    success: (res) => {
                        uni.getUserInfo({
                            provider: value,
                            success: (infoRes) => {
                                /**
                                 * 实际开发中，获取用户信息后，需要将信息上报至服务端。
                                 * 服务端可以用 userInfo.openId 作为用户的唯一标识新增或绑定用户信息。
                                 */
                                this.toMain(infoRes.userInfo.nickName);
                            }
                        });
                    },
                    fail: (err) => {
                        console.error('授权登录失败：' + JSON.stringify(err));
                    }
                });
            },
            toMain(userName) {
                this.login(userName);
                /**
                 * 强制登录时使用reLaunch方式跳转过来
                 * 返回首页也使用reLaunch方式
                 */
                if (this.forcedLogin) {
                    uni.reLaunch({
                        url: '../main/main',
                    });
                } else {
                    uni.navigateBack();
                }

            }
        },
        onReady() {
            this.initPosition();
            this.initProvider();
        }
    }
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
	
	.login-content{
		padding-left: 40rpx;
		
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
		
	}
    
</style>
