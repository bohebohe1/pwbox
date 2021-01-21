<template>
	<view>
		{{id}}
	</view>
</template>

<script>
	import {selectInformationType} from '../../sqlite.js'
	export default {
		data() {
			return {
				id: ''
			}
		},
		onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		        console.log(option); //打印出上个页面传递的参数。
				this.id = option.id
		    },
		methods: {
			init(){
				selectInformationType()
			}
		}
	}
</script>

<style>

</style>
