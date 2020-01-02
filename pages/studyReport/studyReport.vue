<template>
	<view class="studyReportContent">
		<text class="report-time">学习报告时间更新至2019-12-07 11:07:29</text>
		<view class="days" @click="showModal" data-target="Modal">
			【MBA大师】陪我度过<text>1</text>天
			<i class="cuIcon-question text-grey"></i>
		</view>
		<neil-modal :show="isShow" @close="closeModal" title="报告指南">
			<text class="modal_info">
				1.最新数据：进入页面后请下拉刷新获取最新数据（刷新间隔请大于2分钟）。
				2.陪度时间：自注册时间开始记录；
				3.连续学习天数：每天观看视频总时长≥30分钟或者每天做题数≥10个，记为一天。
				4.视频学习：
				1）自使用4.0版本开始统计
				2）观看单节视频时长≥1min时，开始记录数据
				5.做题统计：
				1）统计最近半年的做题数据。
				2）题目均为客观题，主观题不计入统计
				6.预估分说明：
				1）预估分是指在题库做题得分的一个动态参考值（不含主观题），一般与做题的正确率、做题量和做题知识点覆盖范围有关。
				2）预估分是技术上运用复杂的算法计算得出，当单次练习的正确率高于当前预估分，则预估分会增加
				3）预估分需要满足的做题量：
				  逻辑：做题量≥30
				  数学：做题量≥25
				  英语：完型≥1篇且阅读理解A≥2篇且阅读理解B≥1篇3个条件同时满足
				7.击败用户：在全站参与评估分的用户中，低于您的排名的用户百分比，也是一个动态参考值
			</text>
		</neil-modal>
		<view class="flex item-list">
			<view class="item">
				<text class="block">
					<text class="text-xxl">0</text>
					天</text>
				<text>连续学习</text>
			</view>
			<view class="item">
				<text class="block">
					<text class="text-xxl">0</text>
					小时</text>
				<text>视频学习</text>
			</view>
			<view class="item">
				<text class="block">
					<text class="text-xxl">0</text>
					个</text>
				<text>做题总数</text>
			</view>
		</view>
		<view class="solid-bottom line"></view>
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
					<text class="ml20 mt10"><i class="cuIcon-info text-orange mr20"></i>最近7天视频学习时长</text>
					<canvas canvas-id="canvasLineA" id="canvasLineA" class="charts" @touchstart="touchLineA" width="750" height="500"
					 style="width:750rpx;height:500rpx;"></canvas>
				</view>
				<view class="flex justify-between">
					<text class="ml20 mt10"><i class="cuIcon-myfill text-blue mr20"></i>今日视频学习总时长 </text><text class="mt10 mr20 text-bold">0分钟</text></text>
				</view>
				<view class="flex justify-between">
					<text class="ml20 mt10"><i class="cuIcon-time text-green mr20"></i>视频学习总时长 </text><text class="mt10 mr20 text-bold">0分钟</text></text>
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
	import neilModal from '@/components/neil-modal/neil-modal.vue';
	var _self;
	var canvaLineA = null;
	export default {
		components: {
			clTabs,
			uCharts,
			neilModal
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
				isShow: false,
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
			showModal() {
				this.isShow = true;
			},
			closeModal() {
				this.isShow = false
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
	.modal_info{
		padding: 40upx;
		height: 500upx;
		overflow-y: scroll;
		display: block;
	}
	.mr20{
		margin-right: 10upx;
	}
	.ml20{
		margin-left: 10upx;
	}
	.mt10{
		margin-top: 50upx;
		display: block;
	}
	.studyReportContent {
		width: 100%;
		background-color: #fff;
		.report-time{
			padding-left: 20upx;
			color: #CCCCCC;
		}
		.line{
			margin: 25upx 0px;
			height: 1px;
			background-color: #AAAAAA;
		}
		.item-list{
			height: 200upx;
			background-color: #0081FF;
			border-radius: 20upx;
			margin:0px 20upx;
			color: #fff;
			.item{
				text-align: center;
				flex:1;
				align-self: center;
			}
		}
		.days{
			font-size: 30upx;
			margin: 30upx 20upx;
			text{
				color: red;
			}
			i{
				margin-left: 12upx;
			}
		}
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
