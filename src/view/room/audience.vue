<template>
	<el-row>
		<h2 class="room_title">直播间名字-用户管理</h2>
		<div class="info">
			<el-col class="info_item" :span="8">总观看人数: {{totalnum}}</el-col>
			<el-col class="info_item" :span="8">总播放时长: {{totaltime}}min</el-col>
			<el-col class="info_item" :span="8">开播时间: 2016-12-05 09:14</el-col>
		</div>
		<div class="hr"></div>
		<el-table :data="tableData" stripe>
			<el-table-column prop="id" label="序号"></el-table-column>
			<el-table-column prop="user_name" label="昵称"></el-table-column>
			<!--<el-table-column prop="userid" label="手机号"></el-table-column>-->
			<el-table-column prop="watch_times" label="总观看时长(min)"></el-table-column>
			<el-table-column prop="latest_out_time" label="最后进入时间"></el-table-column>
			<el-table-column prop="sex" label="性别"></el-table-column>
			<el-table-column prop="province" label="地区"></el-table-column>

		</el-table>

		<div class="control">
			<el-pagination class="audience_total" layout='prev,pager,next' :page-size='5' :total='setMaxPage' @current-change='handleCurrentChange'>
			</el-pagination>
			 <el-button class="export">邮件发送</el-button> 
			<el-button class="export" type="primary">保存到本地</el-button>
		</div>
	</el-row>
</template>
<script>
	export default {
		data: function() {
			return {
				//表格数据
				tableData: [],
				//总观看人数
				totalnum:'',
				//总直播时长
				totaltime:'',
				//开播时间
				startime:'',
				//分页最大数
				maxpage:''
			}
		},
		computed:{
			//由于分页total只能是数字
			setMaxPage:function(){
				return parseInt(this.maxpage)
			}
		},
		methods: {
			// handleSizeChange:function(val) {
			// 	console.log(`每页 ${val} 条`);
			// },
			// handleCurrentChange:function(val) {
			// 	this.currentPage = val;
			// 	console.log(`当前页: ${val}`);
			// },
			//拿到用户管理列表
			getList:function(){
				this.$http.get('/analyzes/audience/room/'+this.$route.params.id+'?page=1&per-page=5&access-token='+localStorage.getItem('accessToken')).then((response)=>{
					console.log('success')
					console.log(response.body)
					this.totalnum=response.body.data.watch_num
					this.tableData=response.body.data.list
					this.maxpage=response.body.data.pageInfo.totalCount
					for(var a=0;a<this.tableData.length;a++){
						if(this.tableData[a].sex==null){
							this.tableData[a].sex='男'
						}else if(this.tableData[a].sex==1){
							this.tableData[a].sex='男'
						}else{
							this.tableData[a].sex='女'
						}
					}
					this.$http.post('/analyzes/data/room/'+this.$route.params.id,{time_slot:0}).then((response)=>{
						console.log('success')
						console.log(response.body)
						this.totaltime=response.body.data.situation.live_duration
					},(response)=>{
						console.log('error')
						console.log(response.body)
					})
				},(response)=>{
					console.log('error')
					console.log(response.body)
				})
			},
			//当前页改变时的函数
			handleCurrentChange:function(val){
				console.log(val)
				this.$http.get('/analyzes/audience/room/'+this.$route.params.id+'?page='+val+'&per-page=5&access-token='+localStorage.getItem('accessToken')).then((response)=>{
					console.log('success')
					console.log(response.body)
					this.tableData=response.body.data.list
					for(var a=0;a<this.tableData.length;a++){
						if(this.tableData[a].sex==null){
							this.tableData[a].sex='男'
						}else if(this.tableData[a].sex==1){
							this.tableData[a].sex='男'
						}else{
							this.tableData[a].sex='女'
						}
					}
				},(response)=>{
					console.log('error')
					console.log(response.body)
				})
			}
		},
		mounted(){
			this.getList()
		}
	}
</script>
<style lang="less">
	.room_title {
		text-align: center;
		padding: 15px 0;
	}
	
	.info {
		overflow: hidden;
		.info_item {
			text-align: center;
		}
	}
	
	.control {
		text-align: right;
		margin-top: 15px;
		padding-top: 5px;
		border-top: 1px solid #c0c0c0;
		// 统计分页
		.audience_total {
			display: inline-block;
		}
		.export {
			float: left;
		}
	}
</style>