<template>
	<view class="all">
		<view class="status_bar">
			<!-- 这里是状态栏 -->
		</view>
		<view class="addpsd">
			<text>新增密码</text>
		</view>
		<!-- <view class="main">
			<input class="titleinput" type="text" placeholder="请输入标题" :value="title" @input="titlefn"/>
			<input class="titleinput" type="text" placeholder="输入用户名" :value="username" @input="usernamefn" />
			<input class="titleinput" type="text" placeholder="请输入密码" :value="password" @input="passwordfn" />
			<textarea class="titleinput" cols="2" rows="6" style="overflow:hidden;height:300rpx;" placeholder="请输入备注" @input="ramarkfn" :value="ramark"></textarea>
			<view class="sumbitbtn" @click="submitbtn"> 
				添加密码
			</view>
		</view> -->
		<view class="editForm" >
			<view class="edittype">
				<text class="typelabel">请选择类型</text>
				<picker mode="selector" :value="type" :range-key="'Name'" :range="expressTemplate" @change="expressTemplateChange">
					  <view>{{expressTemplate[type].Name}}</view>
				</picker>
			</view>
				
		</view>
		<view class="editForm">
			<input class="titleinput" type="text" placeholder="请输入标题" :value="title" @input="titlefn"/>
		</view>
		<view class="editForm" style="height: 200rpx;">
			<input class="titleinput" style="border-bottom:#D3D3D3 2rpx solid; ;" type="text" placeholder="输入用户名" :value="username" @input="usernamefn" />
			<input class="titleinput" type="text" placeholder="请输入密码" :value="password" @input="passwordfn" />
		</view>
		
		<view class="editForm" style="height: 200rpx;">
			<textarea class="titleinput" cols="2" rows="6" style="overflow:hidden;height:200rpx;line-height: 50rpx;" placeholder="请输入备注" @input="ramarkfn" :value="ramark"></textarea>
		</view>
		<view class="sumbitbtn" @click="submitbtn">
			添加密码
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
				Storage_data: [],
				Name: '',
				expressTemplate: [{Name: '游戏'}, {Name: '工作'} , {Name: '其他'}],
				type: 0,
				
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
			// 密码类型改变
			expressTemplateChange(e) {
				this.type = e.detail.value
				console.log(e.detail)
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
				console.log(this.type)
				// 存储数据
				addUserInformation({
					type: String(this.type),
					id:this.title,
					username: this.username,
					password: this.password,
					ramark:this.ramark,
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

<style lang="scss">
	.all{
		width: 100%;
		height: 100vh;
		background: #F8F8FA;
		.editForm{
			width: 90%;
			height: 100rpx;
			background: #ffffff;	
			margin: 50rpx auto 0 auto;
			border-radius: 30rpx;
		}
	}
	.status_bar{
		height: 40rpx;
		width: 100%;
	}
	.edittype{
		display: flex;
		width: 90%;
		height: 100rpx;
		margin:0 auto;
		align-items: center;
		.typelabel{
			color: #808080;
			font-size: 26rpx;
		}
		picker{
			width: 50%;
			margin-left: 80rpx;
			text-align: center;
		}
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
	}
	.main{
		padding: 0 10rpx;
	}
	.titleinput{
		font-size: 26rpx;
		width: 90%;
		height: 100rpx;
		margin: 0 auto;
		// border-bottom: 	#D3D3D3 2rpx solid;
	}
	.sumbitbtn{
		width: 90%;
		height: 80rpx;
		background: #007AFF;
		border-radius: 20rpx;
		text-align: center;
		color: white;
		line-height: 80rpx;
		margin: 40rpx auto 0 auto; 
	}
</style>
