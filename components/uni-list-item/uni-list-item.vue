<template>
  <view
    :class="disabled ? 'uni-list-item--disabled' : ''"
    :hover-class="disabled || showSwitch ? '' : 'uni-list-item--hover'"
    class="uni-list-item"
    @click="onClick">
	<view class="flex">
		<view class="flex-four">
			<view class="uni-list-item__container">
			  <view class="uni-list-item__content">
				  <view class="blueRound"></view>
				  <view class="uni-list-item__content-title">{{ title }}</view>
			  </view>
			</view>
			<view class="flex margin-top-sm progress">
				<view class="cu-progress sm round">
					<view class="bg-blue" :style="{width:(curryNum/allNum)*100+'%'}"></view>   
				</view>
				<text class="margin-left text-sm text-grey">{{curryNum}}/{{allNum}}</text>
				<text class="margin-left text-sm text-grey" style="    vertical-align: top;margin-top: -5rpx;min-width: 100px;">正确率:{{(haveSure/allNum * 100).toFixed(2)}}%</text>
			</view>
		</view>
		<view class="flex-sub margin-top">
			<text class="cuIcon-edit text-grey edit-button"></text>
		</view>
	</view>
  </view>
</template>

<script>
import uniIcon from '../uni-icon/uni-icon.vue'
import uniBadge from '../uni-badge/uni-badge.vue'
export default {
  name: 'UniListItem',
  components: {
    uniIcon,
    uniBadge
  },
  props: {
	  curryNum: {
		// 当前阅读数
		type: [Number, String],
		default: 0
	  },
	  allNum: {
		// 阅读总数
		type: [Number, String],
		default: 100
	  },
	  haveSure:{
		// 正确数
		type: [Number, String],
		default: 0  
	  },
    title: {
      type: String,
      default: ''
    }, // 列表标题
    note: {
      type: String,
      default: ''
    }, // 列表描述
    disabled: {
      // 是否禁用
      type: [Boolean, String],
      default: false
    },
    showArrow: {
      // 是否显示箭头
      type: [Boolean, String],
      default: true
    },
    showBadge: {
      // 是否显示数字角标
      type: [Boolean, String],
      default: false
    },
    showSwitch: {
      // 是否显示Switch
      type: [Boolean, String],
      default: false
    },
    switchChecked: {
      // Switch是否被选中
      type: [Boolean, String],
      default: false
    },
    badgeText: {
      // badge内容
      type: String,
      default: ''
    },
    badgeType: {
      // badge类型
      type: String,
      default: 'success'
    },
    thumb: {
      // 缩略图
      type: String,
      default: ''
    },
    showExtraIcon: {
      // 是否显示扩展图标
      type: [Boolean, String],
      default: false
    },
    extraIcon: {
      type: Object,
      default () {
        return {
          type: 'contact',
          color: '#000000',
          size: 20
        }
      }
    }
  },
  data () {
    return {}
  },
  methods: {
    onClick () {
      this.$emit('click')
    },
    onSwitchChange (e) {
      this.$emit('switchChange', e.detail)
    }
  }
}
</script>

<style lang="scss" scoped>
@mixin list-disabled {
	opacity: 0.3;
}

$list-item-pd: $uni-spacing-col-lg $uni-spacing-row-lg;
.progress{
	width: 80%;
	margin-left: 60rpx;
}
.blueRound{
	display: inline-block;
	width: 15rpx;
	height: 15rpx;
	background-color: dodgerblue;
	vertical-align: top;
	border-radius: 50%;
	margin-top: 14rpx;
	margin-right: 20rpx;
}
.edit-button{
	position: absolute;
	right: 60rpx;
    top: 44rpx;
	font-size: 50rpx;
}
.uni-list-item {
	font-size: $uni-font-size-lg;
	position: relative;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	margin-top: 10rpx;
	height: 150rpx;
	.uni-list-item--disabled {
		@include list-disabled;
	}

	.uni-list-item__container {
		margin: $list-item-pd;
		width: 100%;
		box-sizing: border-box;
		flex: 1;
		position: relative;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}

	.uni-list-item__content {
		flex: 1;
		display: flex;
		color: #3b4144;
		.uni-list-item__content-title {
			font-size: $uni-font-size-lg;
			text-overflow: ellipsis;
			white-space: nowrap;
			color: inherit;
			line-height: 1.5;
			overflow: hidden;
			display: inline-block;
		}

		// &-note {
		// 	margin-top: 6upx;
		// 	color: $uni-text-color-grey;
		// 	font-size: $uni-font-size-base;
		// 	white-space: normal;
		// 	display: -webkit-box;
		// 	-webkit-box-orient: vertical;
		// 	-webkit-line-clamp: 2;
		// 	overflow: hidden;
		// }
	}

	// &__extra {
	// 	width: 25%;
	// 	display: flex;
	// 	flex-direction: row;
	// 	justify-content: flex-end;
	// 	align-items: center;
	// }

	// &__icon {
	// 	margin-right: 18upx;
	// 	display: flex;
	// 	flex-direction: row;
	// 	justify-content: center;
	// 	align-items: center;

	// 	&-img {
	// 		height: $uni-img-size-base;
	// 		width: $uni-img-size-base;
	// 	}
	// }
}

.uni-list > .uni-list-item:last-child .uni-list-item-container:after {
	height: 0px;
}
</style>
