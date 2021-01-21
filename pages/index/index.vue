<template>
	<view class="">
		<view class="status_bar">
			<!-- 这里是状态栏 -->
		</view>
		<view class="addpsd">
			<text style="margin-left: 10rpx;">密码盒</text>
			<image src="/static/add.png" @click="addpassword" mode="" style="display:block;width:48rpx;height: 48rpx;margin-right: 10rpx;"></image>
		</view>
		<view class="contain">
			<view v-for="item in storagedata" class="search" :style="{backgroundColor: bg()}" @click="editpw(item.list)">
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
			</view>
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
					console.log(res,'3')
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
	.contain{
		
	}
	.search{
		width: 95%;
		height: 50px;
		border-top-left-radius: 10px;
		border-top-right-radius: 10px;
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
