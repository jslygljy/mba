<template>
	<view class="">
		<view class="cu-bar bg-white margin-top">
			<view class="action">
			</view>
			<view class="action">
				<button class="cu-btn bg-blue shadow margin-left" @tap="showModal" data-target="DrawerModalR">筛选</button>
			</view>
		</view>
		<view class="cu-modal drawer-modal justify-end" :class="modalName=='DrawerModalR'?'show':''" @tap="hideModal">
			<view class="cu-dialog basis-xl info-y" :style="[{top:CustomBar+'px',height:'calc(100vh - ' + CustomBar + 'px)'}]">
				<view class="cu-list menu text-left">
					<view class="selectInfo" v-for="(item, index) in list" :key="index">
						<h3>{{item.name}}</h3>
						<view class="selectList">
							<text :class="['selectItem', subitem.isCheck?'selectLight':'']" 
							 v-for="(subitem, subindex) in item.listInfo" :key="subindex"
							 @click="toggleIndex(index,subindex)"
							>{{subitem.name}}</text>
						</view>
					</view>
					<view class="flex">
						<button class="cu-btn round line-blue flex-sub margin" @click="reset">重置</button>
						<button class="cu-btn round bg-blue flex-sub margin" @click="getList2">确定</button>
					</view>
				</view>
			</view>
		</view>
		<uni-list class="list-info">
			<view class="flex solids-bottom lists" @click="goToDetail(item.innerid)" v-for="(item, index) in list2" :key="index">
				<view class="flex-sub">
					<image :src="item.logo" mode="" style="width: 150rpx;height: 150rpx;margin: 0px 12rpx;"></image>
				</view>
				<view class="flex-sub">
					<view class="uni-list-item__container">
					  <view class="uni-list-item__content">
						  <view class="title2">{{ item.name }}</view>
					  </view>
					  <view class="uni-list-item__container">
					  	<view class="title">创办时间: {{item.establish_time}}</view>				  
					  </view>
					</view>
				</view>
				<view class="flex-sub">
					<view style="margin: 0px 20rpx">
						<uni-badge :text="item.nature" type="primary" style="margin-top: 60px;display: inline-block;margin-right: 9px;"></uni-badge>
					</view>
				</view>
			</view>
		</uni-list>
	</view>
</template>

<script>
	import uniList from '@/components/uni-list/uni-list.vue';
	import config from '../../config.js';
	import uniBadge from "@/components/uni-badge/uni-badge.vue"
	const initData = [{
					name:'学制',
					listInfo:[{
						name:'全日制',
						isCheck:false
					},{
						name:'半全日制',
						isCheck:false
					}]
				},{
					name:'性质',
					listInfo:[{
						name:'985',
						isCheck:false
					},{
						name:'211',
						isCheck:false
					},{
						name:'双一流',
						isCheck:false
					},{
						name:'2011计划',
						isCheck:false
					},{
						name:'111计划',
						isCheck:false
					}]
				},{
					name:'类型',
					listInfo:[{
						name:'MBA',
						isCheck:false
					},{
						name:'MPACC',
						isCheck:false
					},{
						name:'MPA',
						isCheck:false
					},{
						name:'MAUD',
						isCheck:false
					},{
						name:'MEM',
						isCheck:false
					},{
						name:'MTA',
						isCheck:false
					},{
						name:'MLIS',
						isCheck:false
					}]
				},{
					name:'地区',
					listInfo:[{
						name:'北京',
						isCheck:false
					},{
						name:'天津',
						isCheck:false
					},{
						name:'河北',
						isCheck:false
					},{
						name:'山西',
						isCheck:false
					},{
						name:'内蒙古',
						isCheck:false
					},{
						name:'辽宁',
						isCheck:false
					},{
						name:'吉林',
						isCheck:false
					},{
						name:'黑龙江',
						isCheck:false
					},{
						name:'上海',
						isCheck:false
					},{
						name:'江苏',
						isCheck:false
					},{
						name:'浙江',
						isCheck:false
					},{
						name:'安徽',
						isCheck:false
					},{
						name:'福建',
						isCheck:false
					},{
						name:'江西',
						isCheck:false
					},{
						name:'山东',
						isCheck:false
					},{
						name:'河南',
						isCheck:false
					},{
						name:'河北',
						isCheck:false
					},{
						name:'湖北',
						isCheck:false
					},{
						name:'湖南',
						isCheck:false
					},{
						name:'广东',
						isCheck:false
					},{
						name:'广西',
						isCheck:false
					},{
						name:'河南',
						isCheck:false
					},{
						name:'重庆',
						isCheck:false
					},{
						name:'西川',
						isCheck:false
					},{
						name:'贵州',
						isCheck:false
					}]
				}];
	export default {
		components: {
			uniList,
			uniBadge
		},
		data() {
			return {
				list:JSON.parse(JSON.stringify(initData)),
				pageindex:0,
				modalName: null,
				CustomBar: this.CustomBar,
				edu_system:[],
				item_category:[],
				area:[],
				list2: [ 
					{ 
						"innerid": "bc79272b-146b-4a24-84c4-9cd02951e943", 
						"name": "内蒙古科技大学土木工程学院MEM项目", 
						"logo": "http://47.104.64.220/public/upload/ec581336-03b4-475f-a4f3-137e740eb4c1.png", 
						"establish_time": "1988", 
						"nature": "" 
						}, 
						{ 
						"innerid": "3b8e8f3f-dc0e-4d4e-bbd1-56fa80c724e5", 
						"name": "清华大学五道口金融学院MBA项目", 
						"logo": "http://47.104.64.220/public/upload/cf8d8109-7edd-4f85-a700-d6abc2d60375.png", 
						"establish_time": "2015", 
						"nature": "985"
					} 
				] 
			}
		},
		onLoad() {

		},
		onShow() {
			this.getList();

		},
		methods: {
			getList() {
				this.list.map((data)=>{
					if(data.name=='地区'){
						data.listInfo.map((data2)=>{
							if(data2.isCheck){
								this.area.push(data2.name)
							}
						})
					}
					if(data.name=='类型'){
						data.listInfo.map((data2)=>{
							if(data2.isCheck){
								this.item_category.push(data2.name)
							}
						})
					}
					if(data.name=='学制'){
						data.listInfo.map((data2)=>{
							if(data2.isCheck){
								this.edu_system.push(data2.name)
							}
						})
					}
				})
				let id = uni.getStorageSync('customer_id');
				// 推荐课程
				uni.request({
					url: config.url + '/app/uc/list?pageindex='+this.pageindex, //仅为示例，并非真实接口地址。
					method:"POST",
					data: {
						edu_system:this.edu_system,
						item_category:this.item_category,
						area:this.area
					},
					success: (res) => {
						console.log(res);
					}
				});
			},
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			onClick(){
				
			},
			toggleIndex(index,subindex){
				this.list[index]['listInfo'][subindex].isCheck = !this.list[index]['listInfo'][subindex].isCheck;
			},
			getList2(){
				this.hideModal();
				this.getList();
			},
			goToDetail(innerid){
				uni.navigateTo({
				    url: '../ucDetail/ucDetail?innerid='+innerid
				});
			},
			reset(){
				this.list.map((data)=>{
					data.listInfo.map((data2)=>{
						data2.isCheck = false
					})
				})
			}
			
		}

	}
</script>

<style lang="scss">
@import "../../static/icon.css";
@import "../../static/main.css";
.info-y{
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
	font-size: 32rpx;
	line-height: 1.5;
	overflow: hidden;
	display: inline-block;
	margin-top: 30rpx;
	margin-bottom: 40rpx;
}
.selectInfo{
	padding: 20rpx 0rpx 0px 10px;
	h3{
		font-size: 32rpx;
		font-weight: bold;
	}
	.selectList{
		padding: 20rpx 0rpx 0px 10px;
		.selectItem{
			font-size: 26rpx;
			width: 142rpx;
			margin:5px 10rpx;
			display: inline-block;
			height: 50rpx;
			border-radius: 30rpx;
			background-color: #CCCCCC;
			color: #000;
			line-height: 50rpx;
			text-align: center;
		}
		.selectLight{
			background-color: #B3D4FC;
			color: #0081FF;
		}
	}
}
</style>

