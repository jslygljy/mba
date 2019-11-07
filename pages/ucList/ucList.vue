<template>
	<view class="uclist">
		<view class="cu-modal" :class="modalName=='DrawerModalR'?'show':''" @click="hideModal">
		</view>
		<view class="drawer-modal cu-modal1" :class="modalName=='DrawerModalR'?'show':''" style="height: 100%;">
			<view class="cu-dialog basis-xl info-y" :style="[{top:CustomBar+'px',height:'100%'}]">
				<view class="cu-list menu text-left">
					<view class="selectInfo" v-for="(item, index) in list" :key="index">
						<h3>{{item.name}}</h3>
						<view class="selectList">
							<text :class="['selectItem', subitem.isCheck?'selectLight':'']" v-for="(subitem, subindex) in item.listInfo"
							 :key="subindex" @click="toggleIndex(index,subindex)">{{subitem.name}}</text>
						</view>
					</view>
				</view>
			</view>
			<view class="flex" style="position: absolute;bottom: 0px;background-color: #FFFFFF;text-align: center;width: 100%;height: 80px;">
				<button class="cu-btn round line-blue flex-sub margin" @click="reset">重置</button>
				<button class="cu-btn round bg-blue flex-sub margin" @click="getList2">确定</button>
			</view>
		</view>

		<view class="list-info">
			<mescroll-uni @up="upCallback" :down="downOption" :up="upOption" :fixed="false" bottom="20" @init="mescrollInit">
				<view class="flex solids-bottom lists" @click="goToDetail(item.innerid)" v-for="(item, index) in list2" :key="index"
				 v-if="list2.length>0">
					<view class="flex-sub">
						<image :src="item.logo" mode="" style="width: 110rpx;height: 110rpx;margin: 30rpx 32rpx 10px;"></image>
					</view>
					<view class="flex-sub">
						<view class="uni-list-item__container">
							<view class="uni-list-item__content">
								<view class="title2">{{ item.name }}</view>
							</view>
							<view class="uni-list-item__container">
								<view class="title">创办时间: {{item.establish_time}}</view>
							</view>
							<view class="uni-list-item__container" style="margin-bottom: 14upx;">
								<uni-badge :text="t" type="primary" class="tag" v-for="(t,i) in item.nature.split('，')"></uni-badge>
							</view>
						</view>
					</view>
					<view class="flex-sub">
						<view style="margin: 0px -6rpx">

						</view>
					</view>
				</view>
			</mescroll-uni>
		</view>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import config from '../../config.js';
	import uniBadge from "@/components/uni-badge/uni-badge.vue"
	import MescrollUni from "@/components/mescroll-uni/mescroll-uni.vue";

	const initData = [{
		name: "学制",
		listInfo: [{
			name:'全日制',
			isCheck: false
		},{
			name:'非全日制',
			isCheck: false
		}]
	}, {
		name: '性质',
		listInfo: [{
			name: '985',
			isCheck: false
		}, {
			name: '211',
			isCheck: false
		}, {
			name: '双一流',
			isCheck: false
		}, {
			name: '2011计划',
			isCheck: false
		}, {
			name: '111计划',
			isCheck: false
		}]
	},{
		name: '地区',
		listInfo: [{
			name: '北京',
			isCheck: false
		}, {
			name: '天津',
			isCheck: false
		}, {
			name: '河北',
			isCheck: false
		}, {
			name: '山西',
			isCheck: false
		}, {
			name: '内蒙古',
			isCheck: false
		}, {
			name: '辽宁',
			isCheck: false
		}, {
			name: '吉林',
			isCheck: false
		}, {
			name: '黑龙江',
			isCheck: false
		}, {
			name: '上海',
			isCheck: false
		}, {
			name: '江苏',
			isCheck: false
		}, {
			name: '浙江',
			isCheck: false
		}, {
			name: '安徽',
			isCheck: false
		}, {
			name: '福建',
			isCheck: false
		}, {
			name: '江西',
			isCheck: false
		}, {
			name: '山东',
			isCheck: false
		}, {
			name: '河南',
			isCheck: false
		}, {
			name: '河北',
			isCheck: false
		}, {
			name: '湖北',
			isCheck: false
		}, {
			name: '湖南',
			isCheck: false
		}, {
			name: '广东',
			isCheck: false
		}, {
			name: '广西',
			isCheck: false
		}, {
			name: '河南',
			isCheck: false
		}, {
			name: '重庆',
			isCheck: false
		}, {
			name: '西川',
			isCheck: false
		}, {
			name: '贵州',
			isCheck: false
		}]
	}];
	export default {
		components: {
			uniList,
			uniBadge,
			MescrollUni
		},
		data() {
			return {
				list: JSON.parse(JSON.stringify(initData)),
				pageindex: 0,
				modalName: null,
				CustomBar: this.CustomBar,
				edu_system: [],
				item_category: [],
				area: [],
				list2: [],
				mescroll: null,
				downOption: {
					use: false,
					auto: false
				},
				upOption: {
					use: true,
					auto: true,
					isBounce: false,
					page: {
						num: 0,
						size: 10
					},
					noMoreSize: 1,
					offset: 80
				},
			}
		},
		onLoad() {

		},
		methods: {
			mescrollInit(mescroll) {
				this.mescroll = mescroll;
			},
			upCallback(mescroll) {
				this.getListDataFromNet(mescroll.num, (curPageData) => {
					mescroll.endSuccess(curPageData.length);
					if (mescroll.num == 1) this.list2 = []; //如果是第一页需手动制空列表
					this.list2 = this.list2.concat(curPageData); //追加新数据
				}, () => {
					mescroll.endErr();
				})
			},
			getListDataFromNet(pageNum, successCallback, errorCallback) {
				setTimeout(() => {
					try {
						this.getList(pageNum - 1, successCallback)
					} catch (e) {
						//联网失败的回调
						errorCallback && errorCallback();
					}
				}, 300)
			},

			getList(pageindex, cb) {
				this.area = [];
				this.item_category = [];
				this.edu_system = [];

				this.list.map((data) => {
					if (data.name == '地区') {
						data.listInfo.map((data2) => {
							if (data2.isCheck) {
								this.area.push(data2.name)
							}
						})
					}
					if (data.name == '学制') {
						data.listInfo.map((data2) => {
							if (data2.isCheck) {
								this.item_category.push(data2.name)
							}
						})
					}
					if (data.name == '性质') {
						data.listInfo.map((data2) => {
							if (data2.isCheck) {
								this.edu_system.push(data2.name)
							}
						})
					}
				})
				let id = uni.getStorageSync('customer_id');
				// 推荐课程
				uni.request({
					url: config.url + '/app/uc/list?pageindex=' + pageindex, //仅为示例，并非真实接口地址。
					method: "POST",
					data: {
						edu_system: this.edu_system,
						item_category: this.item_category,
						area: this.area
					},
					success: (res) => {
						cb && cb(res.data.data)
					}
				});
			},
			onNavigationBarButtonTap(e) {
				if (e.index == 0) {
					this.modalName = "DrawerModalR";
				}
			},
			hideModal(e) {
				this.modalName = null
			},
			onClick() {

			},
			toggleIndex(index, subindex) {
				this.list[index]['listInfo'][subindex].isCheck = !this.list[index]['listInfo'][subindex].isCheck;
			},
			getList2() {
				this.hideModal();
				this.mescroll.resetUpScroll()
			},
			goToDetail(innerid) {
				uni.navigateTo({
					url: '../ucDetail/ucDetail?innerid=' + innerid
				});
			},
			reset() {
				this.list.map((data) => {
					data.listInfo.map((data2) => {
						data2.isCheck = false
					})
				})
			}

		}

	}
</script>

<style lang="scss" scoped>
	.uclist {
		width: 100%;
		background-color: #fff;
	}

	.info-y {
		overflow-y: scroll;
	}

	.title {
		font-size: $uni-font-size-lg;
		text-overflow: ellipsis;
		white-space: nowrap;
		color: inherit;
		line-height: 1.5;
		font-size: 24rpx;
		overflow: hidden;
		display: inline-block;
		color: #666666;
	}

	.title2 {
		font-size: $uni-font-size-lg;
		text-overflow: ellipsis;
		white-space: nowrap;
		color: inherit;
		font-size: 28rpx;
		line-height: 1.5;
		overflow: hidden;
		display: inline-block;
		margin-top: 30rpx;
		// margin-bottom: 10rpx;
	}

	.selectInfo {
		padding: 20rpx 0rpx 0px 10px;

		h3 {
			font-size: 32rpx;
			font-weight: bold;
		}

		.selectList {
			padding: 20rpx 0rpx 0px 10px;

			.selectItem {
				font-size: 26rpx;
				width: 142rpx;
				margin: 5px 10rpx;
				display: inline-block;
				height: 50rpx;
				border-radius: 30rpx;
				background-color: #CCCCCC;
				color: #000;
				line-height: 50rpx;
				text-align: center;
			}

			.selectLight {
				background-color: #B3D4FC;
				color: #0081FF;
			}
		}
	}

	.tag {
		margin-right: 8upx;
		display: inline-block;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		overflow-wrap: break-word;
	}
</style>
