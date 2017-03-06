<template>
	<el-row style='display: flex;justify-content: center;'>
		<el-col :span='20'>
			<el-col class='navcontainer' :span="4">
				<!--<el-menu default-active="1" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose">
					<el-menu-item index="1"><i class="el-icon-menu"></i>直播数据</el-menu-item>
					<el-menu-item index="2"><i class="el-icon-setting"></i>商品销售</el-menu-item>
					<el-menu-item index="3"><i class="el-icon-setting"></i>表单数据</el-menu-item>
					<el-menu-item index="4"><i class="el-icon-setting"></i>问卷调查</el-menu-item>
					<el-menu-item index="5"><i class="el-icon-setting"></i>收益分析</el-menu-item>
				</el-menu>-->
				<el-button type='text'><i class='el-icon-menu'></i>直播数据</el-button>
				<el-button type='text' :disabled="true"><i class='el-icon-setting'></i>商品销售</el-button>
				<el-button type='text' :disabled="true"><i class='el-icon-setting'></i>表单数据</el-button>
				<el-button type='text' :disabled="true"><i class='el-icon-setting'></i>问卷调查</el-button>
				<el-button type='text' :disabled="true"><i class='el-icon-setting'></i>收益分析</el-button>
			</el-col>
			<el-col :span="20">
				<div id="main">
					<div :is='analysisTemplate'></div>
				</div>
			</el-col>
		</el-col>
	</el-row>
</template>

<script>
	import Echarts from 'echarts'
	import livePeople from '../../components/analysisTemplate/livepeople.vue'
	import store from 'store'

	export default {
		name: 'analysis',
		store,
		data() {
			return {
				analysisTemplate: 'livePeople'
			}
		},
		components: {
			livePeople
		},
		methods: {
			handleOpen(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClose(key, keyPath) {
				console.log(key, keyPath);
			}
		},
		create() {

		}
	}
</script>

<style lang="less">
	.navcontainer{
		display: flex;
		flex-wrap: wrap;
		.el-button{
			flex-basis: 100%;
			margin-left: 0;
			margin-top:1.5rem;
			text-align:left;
			i{
				margin-right: 0.5rem;
			}
		}
	}
</style>