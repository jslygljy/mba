<template>
    <view class="taskDetail">
		<uni-countdown 
		:showDay="false"
			:day="0" 
			:hour="0" 
			:minute="0" 
			:second="0">
		</uni-countdown>
		<view class="header">
			<text>{{title}}</text>
			<view class="right-progress">
				<text>{{curryIndex}}</text>
				/
				<text>{{allIndex}}</text>
			</view>
		</view>
		<view class="content">
			<text>(单项选择器)福建省大立科技分类考试大减肥快乐圣诞节里看风景昂克赛拉的减肥快乐大数据库了房间卡拉圣诞节疯狂了的萨芬的时刻来访接待室</text>
		</view>
		<view class="ans-list">
			
		</view>
    </view>
</template>

<script>
	import uniCountdown from "@/components/uni-countdown/uni-countdown.vue"
    export default {
		 components: {uniCountdown},
        data() {
			return {
				title:'2013年联考10月真题',
				curryIndex:'1',
				allIndex:'5'
			}   
         },
        methods: {
			objectChange(e){
				this.curryIndex = e.tab.value
			    console.log();
			},
        }
    }
</script>

<style scoped lang="scss">
	@import "../../static/icon.css";
	@import "../../static/main.css";
    .taskDetail{
		width:100%;
		
	}
	
</style>
