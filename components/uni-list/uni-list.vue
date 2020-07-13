<template>
  <view class="uni-list">
    <slot/>
  </view>
</template>
<script>
export default {
  name: 'UniList'
}
</script>
<style lang="scss">
	.uni-list {
		background-color: $uni-bg-color;
		position: relative;
		width: 100%;
		display: flex;
		flex-direction: column;
	}
</style>
