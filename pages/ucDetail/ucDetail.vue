<template>
	<view class="ucDetail">
		
	</view>
</template>

<script>
	import config from '../../config.js';
	export default {
		data() {
			return {
				innerid:''
			};
		},
		onLoad: function (option) {
		   this.innerid = option.innerid;
		},
		onShow(){
			this.getDetail();
		},
		methods: {
			getDetail(){
				// 获取做题列表
				uni.request({
					url: config.url+'/app/uc/detail/'+this.innerid,
				    data: {
				    },
				    success: (res) => {
						console.log(res)
				    }
				});
			},
		}
	}
</script>

<style lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
.ucDetail{
	
}
</style>
