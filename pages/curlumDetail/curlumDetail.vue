<template>
    <view class="curlum-content">
        <video id="myVideo" src="https://dcloud-img.oss-cn-hangzhou.aliyuncs.com/guide/uniapp/%E7%AC%AC1%E8%AE%B2%EF%BC%88uni-app%E4%BA%A7%E5%93%81%E4%BB%8B%E7%BB%8D%EF%BC%89-%20DCloud%E5%AE%98%E6%96%B9%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B@20181126.mp4"
                    @error="videoErrorCallback" controls></video>
		
		<sun-tab :value.sync="index" @change="objectChange" :tabList="tabObjectList" rangeKey="name"></sun-tab>
		<comment></comment>
    </view>
</template>

<script>
	import sunTab from '@/components/sun-tab/sun-tab.vue';
	import comment from '@/components/commoent/uni-comment.vue';
    export default {
		components:{
			sunTab,
			comment
		},
        data() {
            return {
                tabObjectList: [ //对象数组赋值
                    {
                        name: '简介',
                        value: 1
                    },
                    {
                        name: '目录',
                        value: 2
                    },
                    {
                        name: '评论',
                        value: 3
                    }
                ],
            }
        },
        methods: {
            videoErrorCallback: function(e) {
				uni.showModal({
					content: e.target.errMsg,
					showCancel: false
				})
			},
			objectChange(e){
			    console.log('对象数据返回格式');
			    console.log(e);
			},
        }
    }
</script>

<style scoped lang="scss">

</style>
