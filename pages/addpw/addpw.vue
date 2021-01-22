<template>
	<view class="">
		<view class="status_bar">
			<!-- 这里是状态栏 -->
		</view>
		<view class="addpsd">
			<text style="margin-left: 20rpx;">新增密码</text>
		</view>
		<view class="main">
			<input class="titleinput" type="text" placeholder="请输入标题" :value="title" @input="titlefn"/>
			<input class="titleinput" type="text" placeholder="输入用户名" :value="username" @input="usernamefn" />
			<input class="titleinput" type="text" placeholder="请输入密码" :value="password" @input="passwordfn" />
			<textarea class="titleinput" cols="2" rows="6" style="overflow:hidden;height:300rpx;" placeholder="请输入备注" @input="ramarkfn" :value="ramark"></textarea>
			<view class="sumbitbtn" @click="submitbtn"> 
				添加密码
			</view>
		</view>
	</view>
</template>

<script>
	import {openSqlite,userInfoSQL,addUserInformation} from '../../sqlite.js'
	export default {
		data() {
			return {
				title: '',
				username: '',
				password: '',
				ramark: '',
				createdtime: '',
				Storage_data: []
			}
		},
		onShow (){
			this.init()
		},
		methods: {
			init() {
				openSqlite().then(res=>{}),
				userInfoSQL().then(res=>{})
			},
			titlefn:function(e){
				this.title = e.target.value;
			},
			usernamefn:function(e){
				this.username = e.target.value;
			},
			passwordfn:function(e){
				this.password = e.target.value;
			},
			ramarkfn:function(e){
				this.ramark = e.target.value;
			},
			submitbtn() {
				var time = new Date()
				this.createdtime = time.getFullYear() + '年' + (time.getMonth() + 1) + '月' + time.getDate() + '日'
				if(this.title == '' || this.username == '' || this.password == '' || this.ramark == '') {
					uni.showToast({
					    title: '请输入完整内容',
						icon:'none',
						position:'bottom'  
					});
					return
				}
				// 存储数据
				addUserInformation({
					id:this.title,
					name: this.username,
					gender: this.password,
					avatar:this.ramark,
					createdtime: this.createdtime
				}).then(res=>{
					uni.navigateBack({
						delta:1
					});
				})				
			}
		}
	}
</script>

<style>
	.status_bar{
		height: 40rpx;
		width: 100%;
	}
	.addpsd{
		width: 100%;
		height: 40rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: #F1F1F1 2rpx solid;
	}
	.main{
		padding: 0 10rpx;
	}
	.titleinput{
		width: 100%;
		height: 80rpx;
		line-height: 80rpx;
		border-bottom: 	#D3D3D3 2rpx solid;
	}
	.sumbitbtn{
		width: 100%;
		height: 80rpx;
		background: #007AFF;
		border-radius: 20rpx;
		text-align: center;
		color: white;
		line-height: 80rpx;
		margin-top: 20rpx;
	}
</style>
