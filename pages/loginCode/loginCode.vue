<template>
    <view class="login-content">
        <view class="input-group">
            <view class="input-row border">
                <text class="title">已发送4位验证码至{{tel}}</text>
                <wakary-input type="bottom" @finish="finish" style="width: 100%;"></wakary-input>
            </view>
        </view>
        <view class="btn-row">
            <button type="primary" class="primary" @tap="bindLogin">登录</button>
        </view>
        <view class="action-row">
            <navigator url="../login/login">返回上一步</navigator>
        </view>
        <!-- <view class="oauth-row" v-if="hasProvider" v-bind:style="{top: positionTop + 'px'}">
            <view class="oauth-image" v-for="provider in providerList" :key="provider.value">
                <image :src="provider.image" @tap="oauth(provider.value)"></image>
            </view>
        </view> -->
		<!-- 
		<view class="bottom-row">
			登录即代码同意<navigator url="../reg/reg" class="goToHref">《MBA大师用户协议》</navigator>
		</view> -->
    </view>
</template>

<script>
    import service from '../../service.js';
	import wakaryInput from '@/components/wakary-input/wakary-input.vue'
    import mInput from '../../components/m-input.vue'
	import config from '../../config.js';
    export default {
        components: {
            mInput,
			wakaryInput
        },
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		   this.tel = option.tel;
        },
        data() {
            return {
                tel:'',
				code:''
            }
        },
        methods: {
            finish(code){
				this.code = code;
			},
            bindLogin() {
				/**
				 * 客户端对账号信息进行一些必要的校验。
				 * 实际开发中，根据业务需要进行处理，这里仅做示例。
				 */
				if (this.code.length < 4) {
					uni.showToast({
						icon: 'none',
						title: '请输入验证码'
					});
					return;
				}
				 
			   uni.request({
			       url: config.url+'/app/login', //仅为示例，并非真实接口地址。
				   method:"POST",
			       data: {
			           "mobile":this.tel,
					   "pwd":this.code
			   	},
			       success: (res) => {
					   if(res.data.errcode==0){
						   uni.setStorageSync('customer_id', res.data.data.innerid);
						   uni.showToast({
						   	icon: 'none',
						   	title: '登录成功'
						   });
						   uni.reLaunch({
						       url: '../main/main',
						   });
						   
							
					   }else{
						   uni.showToast({
						   	icon: 'none',
						   	title: res.data.errmsg
						   });
						   
					   }
					   
			   		}
			   });
               
            }
			
        },
        onReady() {
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
