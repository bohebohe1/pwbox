<template>
	<view class="all">
		<view class="status_bar">
			<!-- 这里是状态栏 -->
		</view>
		<view class="addpsd">
			<text>密码盒</text>
			<image src="/static/img/add.png" @click="addpassword" mode="" ></image>
		</view>
		<view class="searchview">
			<input type="text" value="" placeholder="搜索账号密码" class="search"/>
		</view>
		<view class="contain">
			<!-- <view v-for="item in storagedata" class="datalist" :style="{backgroundColor: bg()}" @click="editpw(item.list)"> -->
			<!-- <view v-for="item in storagedata" class="datalist" @click="editpw(item.list)">
				<view class="headview">
					<view class="first">
						{{item.id}}
					</view>
					<view class="mainmsg">
						<view class="">
							{{item.id}}
						</view>
						<view class="username">
							{{item.name}}
						</view>
					</view>
				</view>
				<view class="" style="margin-right: 20px;font-size: 12px;">
					{{item.createdtime}}
				</view>
			</view> -->
				<!-- <van-button type="primary">弹出层</van-button> -->
				
					<van-swipe-cell  right-width="65">
						<view class="swipe">
							<view class="swipetop">
								<view class="switopimg">
									<image src="../../static/img/game.png" style="width: 100%;height: 100%;" mode=""></image>
								</view>
								<view class="">
									<text>98273059</text><br/>
									<text>98273059</text>
								</view>
							</view>
							<view class="swipebottom">
								
							</view>
						</view>
						<view slot="right" class="delpw">删除</view>
					</van-swipe-cell>
		</view>
	</view>
</template>

<script>
	import {openSqlite,userInfoSQL,selectInformationType} from '../../sqlite.js'
	export default {
		data() {
			return {
				title: 'Hello',
				searchvalue: '',
				storagedata: [],
				boole: false,
				bgdata: ['#4CD964','#007AFF','#9932CC','#F0E68C','#D2691E','#000000'],
			}
		},
		onLoad() {
			this.iinit()
		},
		onShow(){
			this.init()
		},
		methods: {
			iinit(){
				// 创建表
				openSqlite().then(res=>{
				}),
				userInfoSQL().then(res=>{
				})
				
			},
			init(){
				selectInformationType('userInfo').then(res=>{
					console.log(res)
					this.storagedata = res
				})
			},
			// 添加背景颜色
			bg() {
				return this.bgdata[Math.floor(Math.random()*5)]
			},
			// 添加密码
			addpassword() {
				uni.navigateTo({
					url: '/pages/addpw/addpw',
					animationType: 'zoom-fade-out',
					animationDuration: 1000
				});
			},
			// 修改密码
			editpw(e) {
				uni.navigateTo({
					url: '/pages/editpw/editpw?id='+e,
				});
			},
		}
	}
</script>

<style lang="scss">
	.all{
		width: 100%;
		height: 100vh;
		background: #F8F8FA;
	}
	.status_bar{
		height: 40rpx;
		width: 100%;
	}
	.addpsd{
		width: 100%;
		height: 100rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: #F1F1F1 2rpx solid;
		text{
			font-weight: bold;
			font-size: 34rpx;
			margin-left: 40rpx;
		}
		image{
			display:block;
			width:48rpx;
			height: 48rpx;
			margin-right: 30rpx;
		}
	}
	.search{
		width: 90%;
		height: 60rpx;
		margin: 0 auto;
		border-radius: 40rpx;
		padding-left:20rpx;
		background-color: #dedede;
		font-size: 26rpx;
	}
	.searchview{
		height: 120rpx;
		display: flex;
		align-items: center;
	}
	.swipe{
		width: 90%;
		height: 260rpx;
		margin: 0 auto;
		border-radius: 20rpx;
		background: #fff;
	}
	.swipetop{
		height: 170rpx;
		display: flex;
		align-items: center;
		border-top-radius: 20rpx;
		border-bottom: 1px #F1F1F1 solid;
		.switopimg{
			width: 120rpx;
			height: 120rpx;
		}
	}
	.swipebottom{
		height: 68rpx;
		border-bottom-radius: 20rpx;
	}
	.delpw{
		width: 100rpx;
		height: 260rpx;
		border-radius: 20rpx;
		color: #FFFFFF;
		background: #DD524D;
		text-align: center;
		line-height: 260rpx;
	}
	.contain{
		height:calc(100vh - 262rpx);
		overflow-y: scroll;
	}
	.datalist{
		width: 90%;
		height: 100px;
		border-top-left-radius: 10px;
		border-top-right-radius: 10px;
		background-color: #ffffff;
		margin: 20px auto 0 auto;
		display: flex;
		justify-content: space-between;
		align-items: center;
		color: #fff;
	}
	.bg{
		background-color: #4CD964;
	}
	.headview{
		display: flex;
		align-items: center;
		margin-left: 20px;
	}
	.first{
		width: 30px;
		height: 30px;
		line-height: 30px;
		border-radius: 30px;
		border: 1px #fff solid;
		font-size: 20px;
		text-align: center;
		overflow: hidden;
	}
	.mainmsg{
		margin-left: 10px;
		display: flex;
		justify-content: flex-start;
		flex-direction: column;
	}
	.username{
		font-size: 12px;
	}
</style>
