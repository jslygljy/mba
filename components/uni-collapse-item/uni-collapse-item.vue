<template>
  <view
    :class="['uni-collapse-cell', { 'uni-collapse-cell--disabled': disabled, 'uni-collapse-cell--open': isOpen }]"
    :hover-class="disabled ? '' : 'uni-collapse-cell--hover'">
    <view
      class="uni-collapse-cell__title header"
      @click="onClick">
	  <view
	    :class="{ 'uni-active': isOpen, 'uni-collapse-cell--animation': showAnimation === true }"
	    class="uni-collapse-cell__title-arrow">
	    <text class="cuIcon-unfold bg-blue" style="margin-top:0rpx;display: inline-block;padding: 8rpx; vertical-align: top;border-radius: 50%;text-align: center;"></text>
	  </view>
     
      <view class="uni-collapse-cell__title-inner">
        <view class="uni-collapse-cell__title-text">{{ title }}</view>
      </view>
      <text class="cuIcon-edit text-grey edit-button"></text>
	  
    </view>
	<view class="flex margin-top progress">
		<view class="cu-progress sm round">
			<view class="bg-blue" :style="[{ width:(curryNum/allNum)}]"></view>
		</view>
		<text class="margin-left text-sm text-grey">{{curryNum}}/{{allNum}}</text>
		<text class="margin-left text-sm text-grey" style="    vertical-align: top;margin-top: -5rpx;min-width: 100px;">正确率:{{(haveSure/allNum * 100).toFixed(2)}}%</text>
	</view>
    <view
      :class="{ 'uni-collapse-cell--animation': showAnimation === true }"
      :style="{ height: isOpen ? height : '0px' }"
      class="uni-collapse-cell__content">
      <view :id="elId">
        <slot />
      </view>
    </view>
	
  </view>
</template>

<script>
import uniIcon from '../uni-icon/uni-icon.vue'
export default {
  name: 'UniCollapseItem',
  components: {
    uniIcon
  },
  props: {
    title: {
      // 列表标题
      type: String,
      default: ''
    },
	curryNum: {
	  // 当前阅读数
	  type: [Number, String],
	  default: 0
	},
	haveSure:{
		// 正确数
		type: [Number, String],
		default: 0  
	},
	allNum: {
	  // 阅读总数
	  type: [Number, String],
	  default: 100
	},
    name: {
      // 唯一标识符
      type: [Number, String],
      default: 0
    },
    disabled: {
      // 是否禁用
      type: [Boolean, String],
      default: false
    },
    showAnimation: {
      // 是否显示动画
      type: Boolean,
      default: false
    },
    open: {
      // 是否展开
      type: [Boolean, String],
      default: false
    },
    thumb: {
      // 缩略图
      type: String,
      default: ''
    }
  },
  data () {
    const elId = `Uni_${Math.ceil(Math.random() * 10e5).toString(36)}`
    return {
      isOpen: false,
      height: 'auto',
      elId: elId
    }
  },
  watch: {
    open (val) {
      this.isOpen = val
    }
  },
  inject: ['collapse'],
  created () {
    this.isOpen = this.open
    this.nameSync = this.name ? this.name : this.collapse.childrens.length
    this.collapse.childrens.push(this)
    if (String(this.collapse.accordion) === 'true') {
      if (this.isOpen) {
        let lastEl = this.collapse.childrens[this.collapse.childrens.length - 2]
        if (lastEl) {
          this.collapse.childrens[this.collapse.childrens.length - 2].isOpen = false
        }
      }
    }
  },
  // #ifdef H5
  mounted () {
    this._getSize()
  },
  // #endif
  // #ifndef H5
  onReady () {
    this._getSize()
  },
  // #endif
  methods: {
    _getSize() {
    	if (this.showAnimation) {
    		uni.createSelectorQuery()
    			.in(this)
    			.select(`#${this.elId}`)
    			.boundingClientRect()
    			.exec(ret => {
    				this.height = ret[0].height+20 + 'px'
    			})
    	}
    },
    onClick () {
      if (this.disabled) {
        return
      }
      if (String(this.collapse.accordion) === 'true') {
        this.collapse.childrens.forEach(vm => {
          if (vm === this) {
            return
          }
          vm.isOpen = false
        })
      }
      this.isOpen = !this.isOpen
      this.collapse.onChange && this.collapse.onChange()
    }
  }
}
</script>

<style lang="scss" scoped>
@mixin collapse-disabled {
	opacity: 0.3;
}

$collapse-title-pd: $uni-spacing-col-lg $uni-spacing-row-lg;

.uni-collapse-cell {
	position: relative;
	.progress{
		width: 70%;
		margin-left: 84rpx;
	}
	.icon-info{
		color: rgb(255, 255, 255);
		font-size: 15rpx;
		background-color: rgb(0, 129, 255);
		border-radius: 50%;
		text-align: center;
		width: 40rpx;
		height: 40rpx;
		text-indent: 4rpx;
	}
	.edit-button{
		position: absolute;
		right: 40rpx;
		top: 32rpx;
		font-size: 50rpx;
	}
	&--disabled {
		@include collapse-disabled;
	}

	&--animation {
		transition: all 0.3s;
	}

	&__title {
		padding: 20rpx 28rpx;
		width: 100%;
		box-sizing: border-box;
		flex: 1;
		position: relative;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;

		&-extra {
			margin-right: 18upx;
			display: flex;
			flex-direction: row;
			justify-content: center;
			align-items: center;
		}

		&-img {
			height: $uni-img-size-base;
			width: $uni-img-size-base;
		}

		&-arrow {
			width: 45rpx;
			height: 45rpx;
			transform: rotate(0deg);
			transform-origin: 50% 45%;
			margin-right: 20rpx;
			font-size: 24rpx;
			&.uni-active {
				transform: rotate(-180deg);
			}
		}

		&-inner {
			flex: 1;
			overflow: hidden;
			display: flex;
			flex-direction: column;
		}

		&-text {
			font-size: $uni-font-size-lg;
			text-overflow: ellipsis;
			white-space: nowrap;
			color: inherit;
			line-height: 1.5;
			overflow: hidden;
		}
	}

	&__content {
		position: relative;
		width: 100%;
		background: $uni-bg-color;
		overflow: hidden;
		padding-bottom: 40rpx;
		.view {
			font-size: $uni-font-size-base;
		}
	}
}
</style>
