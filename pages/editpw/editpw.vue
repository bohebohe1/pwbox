<template>
	<view>
		<view class="main">
			<input class="titleinput" type="text" placeholder="请输入标题" :value="title" @input="titlefn"/>
			<input class="titleinput" type="text" placeholder="输入用户名" :value="username" @input="usernamefn" />
			<input class="titleinput" type="password" placeholder="请输入密码" :value="password" @input="passwordfn" />
			<textarea class="titleinput" cols="2" rows="6" style="overflow:hidden;height:150px;" placeholder="请输入备注" @input="ramarkfn" :value="ramark"></textarea>
			<view class="sumbitbtn" @click="submitbtn"> 
				修改密码
			</view>
		</view>
	</view>
</template>

<script>
	import {selectInformationType,modifyInformation} from '../../sqlite.js'
	export default {
		data() {
			return {
				id: '',
				title:'',
				username:'',
				password: '',
				ramark: ''
			}
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
				//打印出上个页面传递的参数。
				this.id = option.id
				this.init()
		    },
		methods: {
			init(){
				// 获取指定密码
				selectInformationType('userInfo','list',this.id).then(res=>{
					this.title = res[0].id	
					this.username= res[0].name
					this.password= res[0].gender
					this.ramark= res[0].avatar
				})
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
			// 修改密码
			submitbtn(){
				var time = new Date()
				// this.createdtime = time
				this.createdtime = time.getFullYear() + '年' + (time.getMonth() + 1) + '月' + time.getDate() + '日'
				if(this.title == '' || this.username == '' || this.password == '' || this.ramark == '') {
					uni.showToast({
					    title: '请输入完整内容',
						icon:'none',
						position:'bottom'  
					});
					return
				}
				// 修改
				modifyInformation(
				'userInfo',
				{
					id:'id',
					name:'name',
					gender:'gender',
					avatar:'avatar',
					createdtime:'createdtime',
				},
				{
					id:this.title,
					name:this.username,
					gender:this.password,
					avatar:this.ramark,
					createdtime:this.createdtime
				},
				'list',
				this.id
					).then(res=>{
					uni.navigateBack({
						delta:1
					});
				})
			}
		}
	}
</script>

<style>
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
