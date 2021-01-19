<template>
	<view class="">
		<view class="status_bar">
			<!-- 这里是状态栏 -->
		</view>
		<view class="addpsd">
			<text style="margin-left: 10rpx;">密码盒</text>
			<image src="../../static/add.png" @click="addpassword" mode="" style="display:block;width:48rpx;height: 48rpx;margin-right: 10rpx;"></image>
		</view>
		<view class="search">
			<input type="text" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				searchvalue:''
			}
		},
		onLoad() {

		},
		methods: {
			addpassword() {
				console.log('addadd')
			}
		}
	}
</script>

<style>
	.status_bar{
		height: 20rpx;
		width: 100%;
	}
	.addpsd{
		width: 100%;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: #F1F1F1 2rpx solid;
	}
</style>
