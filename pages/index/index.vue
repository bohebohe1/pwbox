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
			<view v-for="item in storagedata" class="search" :style="{backgroundColor: bg()}">
				<view class="headview">
					<view class="first">
						{{item.title}}
					</view>
					<view class="mainmsg">
						<view class="">
							{{item.title}}
						</view>
						<view class="username">
							{{item.username}}
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
	import {openComDB,executeSQL,dropSQL} from '../../sqlite.js'
	export default {
		data() {
			return {
				title: 'Hello',
				searchvalue: '',
				storagedata: [],
				boole: false,
				bgdata: ['#4CD964','#007AFF','#9932CC','#F0E68C','#D2691E','#000000'],
				netWork: null //网络类型 
			}
		},
		onLoad() {
			this.getNetworkType(); //初始化网络当前状态；  
				uni.onNavigationBarButtonTap((e) => {    
			 //该处是因为的导航栏右边加了两个筛选条件。可以忽略。 不过如果做离线需要筛选的，筛选条件等数据同样需要缓存  
				if (e.index == 1) {  
					this.pickType();  
				} else if (e.index == 0) {  
					this.pickBuild()  
				}  
			})  
		},
		onShow(){
			this.init()
		},
		methods: {
			init(){
				if(uni.getStorageSync('storage_key')){
					this.boole = true
					this.storagedata = JSON.parse(uni.getStorageSync('storage_key'))
					console.log(this.storagedata)
				}else {
					this.boole = false
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
				});
			},
			getNetworkType() {  
				//获取网络信息  
				uni.getNetworkType({  
					success: res => {  
						this.netWork = res.networkType;  
						this.isOpenDB();  
					}  
				})  
			},  
			isOpenDB() {  
				console.log('是否打开数据库');  
				var isOpen = plus.sqlite.isOpenDatabase({  
					name: 'nfc', //数据库的名字  
					path: '_doc/bim-boot.db' //地址  
				});  
				console.log(!isOpen);  

				if (!isOpen) {  
					console.log('Unoepned:' + isOpen);  
					// plus.nativeUI.alert('Unopened!');  
					this.openDB(); //打開DB  
				} else {  
					// plus.nativeUI.alert('Opened!');  
					this.isNet();  
					// this.getLocalType();  
				}  
			},  
			openDB() {  
				//SQLite      
				openComDB('nfc', '_doc/bim-boot.db', res => {  
					console.log('打开数据库');  
					this.isNet();  
				});  
			},  
			isNet(){  
			//网络问题；  
					if (this.netWork == 'wifi' || this.netWork == '4g') {  
			//在有网络情况下，会先情况之前的表，为了防止没有及时更新到数据。update我是嫌麻烦，没这样写。就这样暴力写了。  
						console.log('wifi || 4g ');  
						this.dropTable("pointLists");  
						this.dropTable("codeTypeTable");  
						this.dropTable("codeTable");  
						this.dropTable("statusTable")  
			//删了之后创建表  
						this.createCodeTable();  
						this.createCodeTypeTable();  
						this.createBuildTable();  
						this.createLists();  
						this.createUpateStatus(); // 离线时更新  
			//然后把数据插入进去  
						this.getTabType(); //初始化格式  
	
						this.getPonitType(); //获取code。默认值  
						this.getCodeType(); // 类型下的选择项；  
						this.getBuilds(); // 获取建筑物数据  
	
					} else {  
						//无网络时；  
						console.log('初始化无网络');  
						this.getTabType(); //初始化格式  
						this.locCodeTypeItem();  
						this.selBuildFun();  
					}  
	  //上面贴的drop，create，insert 我会在下面贴出部分代码。不会全部贴.SQL语句不会的建议百度找文档多看下，无非就是增删改查  
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
