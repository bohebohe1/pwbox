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
			<input type="text" :value="value" placeholder="搜索账号密码" class="search" @input="seach"/>
		</view>
		<view class="contain">
		<!-- 测试数据 -->
		<!-- 	<view class="" style="margin-top: 40rpx;" >
					<van-swipe-cell right-width="130">
						<view class="swipe">
							<view class="swipetop">
								<view class="switopimg">
									<image src="../../static/img/game.png" style="width: 100%;height: 100%;" mode=""></image>
								</view>
								<view class="swimessage">
									<text>学习</text><br/>
									<text>aaa</text>
								</view>
								<view class="creattime">
									dadada大大
								</view>
							</view>
							<view class="swipebottom">
								备注：迪卡龙发卡量副科级
							</view>
						</view>
						<view slot="right" class="delpw">删除</view>
					</van-swipe-cell>
			</view>
		-->
	
				<view class="" v-for="item in storagedata" style="margin-top: 40rpx;" >
					<van-swipe-cell right-width="130">
						<view class="swipe">
							<view class="swipetop">
								<view class="switopimg">
									<image :src="item.type | imageurl " style="width: 100%;height: 100%;border-radius: 20rpx;" mode=""></image>
								</view>
								<view class="swimessage">
									<text>{{item.type | typetext}}</text><br/>
									<text>{{item.id}}</text>
								</view>
								<view class="creattime">
									{{item.createdtime}}
								</view>
							</view>
							<view class="swipebottom">
								备注：{{item.ramark}}
							</view>
						</view>
							<view slot="right" style="display: flex;">
								<view class="delpw" style="background-color: #7363DD;" @click="editpw(item.list)">修改</view>
								<view class="delpw" style="margin-left: 20rpx;" @click="delpw(item.list)">删除</view>
							</view>
					</van-swipe-cell>
				</view>
		</view>
	</view>
</template>

<script>
	import {openSqlite,userInfoSQL,selectInformationType,deleteInformationType} from '../../sqlite.js'
	export default {	
		filters:{
			imageurl(val){
				if(val == '0'){
					return '/static/img/game1.png'
				}else if(val == '1'){
					return '/static/img/study.png'
				}else {
					return '/static/img/app.png'
				}
			},
			typetext(val) {
				if(val == '0'){
					return '游戏'
				}else if(val == '1'){
					return '工作'
				}else {
					return '其他'
				}
			}
		},
		data() {
			return {
				title: 'Hello',
				searchvalue: '',
				storagedata: [],
				boole: false,
				bgdata: ['#4CD964','#007AFF','#9932CC','#F0E68C','#D2691E','#000000'],
				imagetype: '',
				value: ''
			}
		},
		onLoad() {
			this.iinit()
		},
		onShow(){
			this.init()
			this.value = ''
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
			// 搜索账号
			seach:function (e){
				if(e.target.value){
					selectInformationType('userInfo','username',e.target.value).then(res=>{
						console.log(res)
						this.storagedata = res
					})
				}else {
					console.log('kong')
					this.init()
				}
				
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
					animationDuration: 100
				});
			},
			// 修改密码
			editpw(e) {
				console.log('edit')
				uni.navigateTo({
					url: '/pages/addpw/addpw?id='+e,
				});
			},
			// 删除密码
			delpw(e){
				console.log('删除')
				deleteInformationType('userInfo','list',e).then(res=>{
					this.init()
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
		font-family: PingFangSC-Regular;
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
	
	.searchview{
		height: 80rpx;
		display: flex;
		align-items: center;
		.search{
			width: 90%;
			height: 60rpx;
			margin: 0 auto;
			border-radius: 40rpx;
			padding-left:20rpx;
			background-color: #F1F1F1;
			font-size: 26rpx;
		}
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
		justify-content: space-around;
		border-top-radius: 20rpx;
		border-bottom: 1px #F1F1F1 solid;
		.switopimg{
			width: 100rpx;
			height: 100rpx;
		}
		.swimessage{
			// margin-right: auto;
			text:first-child{
				font-weight: bold;
				font-size: 28rpx;
			}
			text:last-child{
				color: #C6CAC9;
			}
		}
		.creattime{
			font-size: 24rpx;
			color: #C6CAC9;
		}
	}
	.swipebottom{
		height: 68rpx;
		border-bottom-radius: 20rpx;
		display: flex;
		align-items: center;
		margin-left: 60rpx;
		font-size: 28rpx;
		color: #c6cac9;
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
