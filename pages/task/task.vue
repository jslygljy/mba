<template>
	<view>
		<view v-for="(item, index) in list" :key="index">
			<uni-collapse ref="add" class="warp" @change="change">
				<uni-collapse-item v-for="(sub, key) in item" :key="key" :open="sub.open" :show-animation="sub.showAnimation" :disabled="sub.disabled" :title="sub.subName">
					<template v-if="!sub.type">
						<view class="content">{{ sub.content }}</view>
					</template>
					<template v-else>
						<uni-list>
							<uni-list-item v-for="(list, listIndex) in sub.subList" :key="listIndex" :title="list.title" :note="list.note" :thumb="list.thumb" :show-extra-icon="list.showExtraIcon" :extra-icon="list.extraIcon" :show-switch="list.showSwitch" @switchChange="change" />
						</uni-list>
					</template>
				</uni-collapse-item>
			</uni-collapse>
		</view>
	</view>
</template>

<script>
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item/uni-collapse-item.vue'
	import uniList from '@/components/uni-list/uni-list.vue'
	import uniListItem from '@/components/uni-list-item/uni-list-item.vue'

	export default {
		components: {
			uniCollapse,
			uniCollapseItem,
			uniList,
			uniListItem
		},
		data() {
			
			return {
				list: [
						{
							type: true,
							subName: '折叠列表',
							showAnimation: true,
							subList: [{
									title: '标题文字',
									thumb: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png'
								},
								{
									title: '标题文字',
									note: '描述信息',
									thumb: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png'
								},
								{
									title: '标题文字',
									showExtraIcon: true,
									extraIcon: {
										color: '#4cd964',
										size: '26',
										type: 'image'
									},
									showSwitch: true
								}
							]
						}
					]
			}
		},
		methods: {
			onClick() {
				this.list[1].data[1].subList.push({
					title: '新增',
					thumb: 'https://img-cdn-qiniu.dcloud.net.cn/new-page/uni.png'
				})
				this.$nextTick(() => {
					this.$refs.add[1].resize()
				})
			},
			change(e) {
				console.log(e)
			}
		}
	}
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		background-color: #efeff4
	}

	view {
		font-size: 28upx;
		line-height: inherit
	}

	.example {
		padding: 0 30upx 30upx
	}

	.example-title {
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 32upx;
		color: #464e52;
		padding: 30upx;
		margin-top: 20upx;
		position: relative;
		background-color: #fdfdfd
	}

	.example-title__after {
		position: relative;
		color: #031e3c
	}

	.example-title:after {
		content: '';
		position: absolute;
		left: 0;
		margin: auto;
		top: 0;
		bottom: 0;
		width: 10upx;
		height: 40upx;
		border-top-right-radius: 10upx;
		border-bottom-right-radius: 10upx;
		background-color: #031e3c
	}

	.example .example-title {
		margin: 40upx 0
	}

	.example-body {
		border-top: 1px #f5f5f5 solid;
		padding: 30upx;
		background: #fff
	}

	.example-info {
		padding: 30upx;
		color: #3b4144;
		background: #fff
	}

	.content {
		padding: 30upx;
		background: #f9f9f9;
		color: #666;
	}

	.button {
		font-size: 26upx;
		line-height: 90upx;
	}
</style>