<template>
	<view class="studyReportContent">
		<text>学习报告时间更新至2019-12-07 11:07:29</text>
		<view class="days">
			【MBA大师】陪我度过<text>1</text>天
			<uni-icon type="help"></uni-icon>
		</view>
		<view class="flex solid-bottom">
			<view class="item">
				<text>
					<text class="text-xl">0</text>
					天</text>
				<text>连续学习</text>
			</view>
			<view class="item">
				<text>
					<text class="text-xl">0</text>
					小时</text>
				<text>视频学习</text>
			</view>
			<view class="item">
				<text>
					<text class="text-xl">0</text>
					个</text>
				<text>做题总数</text>
			</view>
		</view>
		<view class="tabsSwtich">
			<view :class="['item',selectIndex==0?'selected':'']" @click="selectClick(0)">
				视频学习
			</view>
			<view :class="['item',selectIndex==1?'selected':'']" @click="selectClick(1)">
				做题统计
			</view>
		</view>
		<swiper :indicator-dots="false" :autoplay="false" :current="selectIndex">
			<swiper-item>
				<view class="swiper-item">
					<text><i class="cuIcon-info text-orange"></i>最近7天视频学习时长</text>
					<canvas canvas-id="canvasLineA" id="canvasLineA" class="charts" @touchstart="touchLineA" width="750" height="500"
					 style="width:750rpx;height:500rpx;"></canvas>
				</view>
				<view class="flex">
					<text><i class="cuIcon-myfill text-blue"></i>今日视频学习总时长 </text><text>0分钟</text></text>
				</view>
				<view class="flex">
					<text><i class="cuIcon-time text-green"></i>视频学习总时长 </text><text>0分钟</text></text>
				</view>
			</swiper-item>
			<swiper-item>
				<view class="swiper-item">
					<text class="title">今日做题</text>
					<view class="item-list">
						<view class="number-item">
							<text>0</text>
							<text>逻辑</text>
						</view>
						<view class="number-item">
							<text>0</text>
							<text>数学</text>
						</view>
						<view class="number-item">
							<text>0</text>
							<text>英语</text>
						</view>
						<view class="number-item">
							<text>0</text>
							<text>总提数</text>
						</view>
					</view>
				</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	import clTabs from '@/components/cl-tabs/cl-tabs.vue'
	import uCharts from '@/components/u-charts/u-charts.js';
	var _self;
	var canvaLineA = null;
	export default {
		components: {
			clTabs,
			uCharts
		},
		onLoad() {
			_self = this;
			//#ifdef MP-ALIPAY
			uni.getSystemInfo({
				success: function(res) {
					if (res.pixelRatio > 1) {
						//正常这里给2就行，如果pixelRatio=3性能会降低一点
						//_self.pixelRatio =res.pixelRatio;
						_self.pixelRatio = 2;
					}
				}
			});
			//#endif
			this.cWidth = uni.upx2px(750);
			this.cHeight = uni.upx2px(500);
			this.getServerData();
		},
		data() {
			return {
				tabBars: [{
						tab: '关注',
						scroll: 0
					},
					{
						tab: '关注',
						scroll: 0
					}
				],
				tabCurrentIndex: -1,
				selectIndex: 0,
				cWidth: '',
				cHeight: '',
				pixelRatio: 1,
				textarea: '',
				dataJson: {}
			}
		},
		mounted() {
			this.tabCurrentIndex = 1
		},
		methods: {
			tabChange(e) {
				this.tabCurrentIndex = e;
			},
			selectClick(index) {
				this.selectIndex = index
			},
			getServerData() {
				let LineA = {
					series: [{
						name: "观看时间：",
						disableLegend: true,
						data: [
							35,
							20,
							25,
							37,
							4,
							20,
							20
						]
					}],
					categories: [
						"今天",
						"12-08",
						"12-07",
						"12-06",
						"12-05",
						"12-04",
						"12-03"
					],
				}
				_self.showLineA("canvasLineA", LineA);
			},

			showLineA(canvasId, chartData) {
				console.log(_self);
				canvaLineA = new uCharts({
					$this: _self,
					canvasId: canvasId,
					type: 'area',
					fontSize: 11,
					padding: [15, 20, 0, 15],
					legend: false,
					dataLabel: true,
					dataPointShape: true,
					background: '#FFFFFF',
					pixelRatio: _self.pixelRatio,
					categories: chartData.categories,
					series: chartData.series,
					animation: false,
					legend: {
						show: false
					},
					xAxis: {
						type: 'grid',
						disableGrid: true,
						dashLength: 10,
						boundaryGap: 'justify',
						splitNumber: 5,
						axisLineColor: "#000",
						format: (val) => {
							console.log(val)
							return this.formatDateTime(val, 'str')
						}
					},
					yAxis: {
						disabled: true,
						disableGrid: true,
					},
					width: _self.cWidth * _self.pixelRatio,
					height: _self.cHeight * _self.pixelRatio,
					extra: {
						area: {
							type: 'curve',
							addLine: true,
							gradient: true
						}
					}
				});

			},
			formatDateTime(timeStamp, returnType) {
				var date = new Date();
				date.setTime(timeStamp * 1000);
				var y = date.getFullYear();
				var m = date.getMonth() + 1;
				m = m < 10 ? ('0' + m) : m;
				var d = date.getDate();
				d = d < 10 ? ('0' + d) : d;
				var h = date.getHours();
				h = h < 10 ? ('0' + h) : h;
				var minute = date.getMinutes();
				var second = date.getSeconds();
				minute = minute < 10 ? ('0' + minute) : minute;
				second = second < 10 ? ('0' + second) : second;
				if (returnType == 'str') {
					return h + ':' + minute;
				}
				return [y, m, d, h, minute, second];
			},
			touchLineA(e) {
				canvaLineA.showToolTip(e, {
					format: function(item, category) {
						return item.name + ':' + item.data
					}
				});
			}
		}
	}
</script>

<style scoped lang="scss">
	.studyReportContent {
		width: 100%;
		background-color: #fff;

		uni-swiper {
			height: 100%;
			display: flex;
			flex-direction: column;
		}

		.charts {
			width: 750upx;
			height: 500upx;
		}

		.tabsSwtich {
			width: 70%;
			margin-left: 15%;
			border: 2upx #808080 solid;
			border-radius: 40rpx;
			display: flex;
			height: 60upx;
			justify-content: space-between;

			.item {
				flex: 1;
				text-align: center;
				line-height: 60upx;
				color: #808080;

				&.selected {
					color: white;
					background-color: #0081FF;
					border-radius: 40rpx;
				}
			}

		}
	}
</style>
