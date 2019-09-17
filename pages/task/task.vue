<template>
	<view class="task-content">
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name"></sun-tab>
		<view class="list">
			<view class="item">
				<text class="cuIcon-punch text-yellow i"></text>
				<text>打印试题</text>
			</view>
			<view class="item" @click="goToRead">
				<text class="cuIcon-newshotfill text-blue i"></text>
				<text>套卷练习</text>
			</view>
			<view class="item">
				<text class="cuIcon-babyfill text-purple i"></text>
				<text>做题历史</text>
			</view>
		</view>
		<text class="practice-title">
			专项练习
		</text>
		<view v-for="(item, index) in list" :key="index" class="collapse-info">
			<uni-collapse @change="change">
				<uni-collapse-item :title="item.title" :show-animation="true">
					<uni-list class="list-info">
						<uni-list-item @click="goToDetail(subitem.id)" v-for="(subitem, subindex) in item.sublist" :key="subindex" :title="subitem.title" :curryNum="subitem.curryNum" :allNum="subitem.allNum">
						</uni-list-item>
					</uni-list>
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
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	export default {
		components: {
			uniCollapse,
			uniCollapseItem,
			uniList,
			uniListItem,
			sunTab
		},
		data() {
			
			return {
				list:[{
					title:'形式逻辑',
					sublist:[{
						id:1,
						title:'前真后假',
						curryNum:10,
						allNum:599
					},{
						id:2,
						title:'逻辑关系or',
						curryNum:1,
						allNum:26
					},{
						id:3,
						title:'充分必要条件',
						curryNum:1,
						allNum:23
					},{
						id:4,
						title:'带入逻辑推命题真假',
						curryNum:2,
						allNum:69
					}]
				},{
					title:'形式逻辑',
					sublist:[{
						id:1,
						title:'前真后假',
						curryNum:10,
						allNum:599
					},{
						id:2,
						title:'逻辑关系or',
						curryNum:1,
						allNum:26
					},{
						id:3,
						title:'充分必要条件',
						curryNum:1,
						allNum:23
					},{
						id:4,
						title:'带入逻辑推命题真假',
						curryNum:2,
						allNum:69
					}]
				}
						
				],
				index: 0,
				tabList: ['逻辑','数学','英语','写作'], //普通数据赋值
				tabObjectList: [ //对象数组赋值
				    {
				        name: '逻辑',
				        value: 1
				    },
				    {
				        name: '数学',
				        value: 2
				    },
				    {
				        name: '英语',
				        value: 3
				    },
				    {
				        name: '写作',
				        value: 4
				    }
				],
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
			},
			objectChange(){
				
			},
			goToDetail(id){
				uni.reLaunch({
				    url: '../taskDetail/taskDetail?id='+id
				});
			}
		}
	}
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
	.task-content{
		height:100%;
		padding-bottom: 100rpx;
		.list{
			display: flex;
			margin-bottom: 20rpx;
			margin-top: 30rpx;
			.item{
				flex:1;
				text-align: center;
				font-size: 24rpx;
				.i{
					display: block;
					font-size: 80rpx;
				}
			}
		}
		.practice-title{
			margin: 40rpx 0px 0rpx 30rpx;
			display: inline-block;
		}
		.list-info{
			margin: 20rpx 0px 0rpx 20rpx;
		}
		.collapse-info{
			margin-top: 25rpx;
		}
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
</style>