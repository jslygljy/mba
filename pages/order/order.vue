<template>
	<view class="orderlist">
		<view class="list-info">
			<view class="flex solids-bottom lists" v-for="(item, index) in list2" :key="index" v-if="list2.length>0">
				<view class="flex-four">
					<view class="uni-list-item__container">
						<view class="uni-list-item__content item">
							<view class="title2">课程名称:{{ item.title }}</view>
						</view>
						<view class="uni-list-item__container item">
							<view class="title">订单号: {{item.pay_id}}</view>
						</view>
						<view class="uni-list-item__container item">
							订单状态：{{filter(item.pay_status)}}
						</view>
					</view>
				</view>
				<view class="flex-sub">
					<view class="uni-list-item__container text-cyan item2 text-xxl">
						{{item.price}}元
					</view>
				</view>
			</view>
			<view class="text-center" v-if="list2.length==0">
				<view class="empty"></view>
				<text class="text-gray block margin-top">暂无订单！</text>
			</view>
		</view>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import config from '../../config.js';
	import uniBadge from "@/components/uni-badge/uni-badge.vue"
	import MescrollUni from "@/components/mescroll-uni/mescroll-uni.vue";

	export default {
		components: {
			uniList,
			uniBadge,
			MescrollUni
		},
		data() {
			return {
				modalName: null,
				CustomBar: this.CustomBar,
				edu_system: [],
				item_category: [],
				area: [],
				list2: [],
			}
		},
		onLoad() {
			this.getList();
		},
		methods: {

			getList() {
				let id = uni.getStorageSync('customer_id');
				// 推荐课程
				uni.request({
					url: config.url + '/app/order/list/' + id,
					data: {},
					success: (res) => {
						// let c = [{"title":"逻辑系统课-形式逻辑","price":1,"memoy":1,"pay_id":"201911191941483157891354","pay_status":0,"time_end":"20191113160725"}];
						let c = res.data.data;
						this.list2 = c;
					}
				});
			},
			filter(data) {
				switch (data) {
					case 0:
						return '未支付'
						break;
					case 1:
						return '支付成功'
						break;
					case 2:
						return '取消支付'
						break;
					default:
						break;
				}
			}

		}

	}
</script>

<style lang="scss" scoped>
	page {
		height: 100%;
	}

	.orderlist {
		height: 100%;
		min-height: 100vh;
		width: 100%;
		background-color: #fff;

		.empty {
			background-image: url('/static/empty_order.png');
			display: inline-block;
			width: 385upx;
			height: 293upx;
			background-size: 100% 100%;
			margin-top: 200upx;
		}
	}

	.item {
		margin: 30upx;
	}

	.item2 {
		margin: 70upx 30upx 30upx;
	}
</style>
