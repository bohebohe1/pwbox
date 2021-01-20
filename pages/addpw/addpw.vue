<template>
	<view class="">
		<view class="status_bar">
			<!-- 这里是状态栏 -->
		</view>
		<view class="addpsd">
			<text style="margin-left: 10rpx;">新增密码</text>
		</view>
		<view class="main">
					<input class="titleinput" type="text" placeholder="请输入标题" :value="title" @input="titlefn"/>
					<input class="titleinput" type="text" placeholder="输入用户名" :value="username" @input="usernamefn" />
					<input class="titleinput" type="password" placeholder="请输入密码" :value="password" @input="passwordfn" />
					<textarea class="titleinput" cols="2" rows="6" style="overflow:hidden;height:150px;" placeholder="请输入备注" @input="ramarkfn" :value="ramark"></textarea>
					<view class="sumbitbtn" @click="submitbtn"> 
						添加密码
					</view>
		</view>
	</view>
</template>

<script>
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
		methods: {
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
				this.createdtime= time.getFullYear() + '年' + (time.getMonth() + 1) + '月' + time.getDate() + '日'
				console.log(this.createdtime)
				if(this.title == '' || this.username == '' || this.password == '' || this.ramark == '') {
					uni.showToast({
					    title: '请输入完整内容',
						icon:'none',
						position:'bottom'  
					});
					return
				}
				if(uni.getStorageSync('storage_key')){
					this.Storage_data = JSON.parse(uni.getStorageSync('storage_key'))
				}else {
					this.Storage_data = []
				}
				this.Storage_data.push({
					title: this.title,
					username: this.username,
					password: this.password,
					ramark: this.ramark,
					createdtime: this.createdtime
				})
				var ss = JSON.stringify(this.Storage_data)
				uni.setStorageSync('storage_key',ss)
				uni.navigateTo({
					url: '/pages/index/index',
				});
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
	.main{
		padding: 0 5px;
	}
	.titleinput{
		width: 100%;
		height: 40px;
		line-height: 40px;
		border-bottom: 	#D3D3D3 1px solid;
	}
	.sumbitbtn{
		width: 100%;
		height: 40px;
		background: #007AFF;
		border-radius: 10px;
		text-align: center;
		color: white;
		line-height: 40px;
		margin-top: 10px;
	}
</style>
