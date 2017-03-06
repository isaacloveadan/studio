<template>
	<div class='livepeople'>
		<el-row>
			<el-col :span='8' :offset='16' style='display: flex;justify-content: flex-end;'>
				<el-button-group>
					<el-button size='mini' :class='{active:isActive1}' @click='chose(1)'>今日</el-button>
					<el-button size='mini' :class='{active:isActive2}' @click='chose(2)'>昨日</el-button>
					<el-button size='mini' :class='{active:isActive3}' @click='chose(3)'>本周</el-button>
					<el-button size='mini' :class='{active:isActive4}' @click='chose(4)'>上周</el-button>
					<el-button size='mini' :class='{active:isActive5}' @click='chose(5)'>本月</el-button>
					<el-button size='mini' :class='{active:isActive6}' @click='chose(6)'>上月</el-button>
				</el-button-group>
				<!--<h6 :class='{active:isActive1}' @click='chose(1)'>今日</h6>
				<h6 :class='{active:isActive2}' @click='chose(2)'>昨日</h6>
				<h6 :class='{active:isActive3}' @click='chose(3)'>本周</h6>
				<h6 :class='{active:isActive4}' @click='chose(4)'>上周</h6>
				<h6 :class='{active:isActive5}' @click='chose(5)'>本月</h6>
				<h6 :class='{active:isActive6}' style="border-right: 1px solid black;" @click='chose(6)'>上月</h6>-->
			</el-col>
		</el-row>
		<el-row class='livecontent' style='margin-top: 0.5rem;'>
			<el-tabs>
				<el-tab-pane label='直播概况'>
					<el-col :span='24'>
						<el-row style='display: flex;justify-content: space-between;'>
							<el-col class='gaiitems' :span='5'>
								<el-row>
									直播时长
								</el-row>
								<el-row class='secitem'>
									<span style='color: #20a0ff;font-size: 2rem;'>{{live_duration}}</span>分钟
								</el-row>
							</el-col>
							<el-col class='gaiitems' :span='5'>
								<el-row>
									观看时长
								</el-row>
								<el-row class='secitem'>
									<span style='color: #20a0ff;font-size: 2rem;'>{{watch_Tduration}}</span>分钟
								</el-row>
							</el-col>
							<el-col class='gaiitems' :span='5'>
								<el-row>
									用户量UV
								</el-row>
								<el-row class='secitem'>
									<span style='color: #20a0ff;font-size: 2rem;'>{{audience_num}}</span>人
								</el-row>
							</el-col>
							<el-col class='gaiitems' :span='5'>
								<el-row>
									观看人次PV
								</el-row>
								<el-row class='secitem'>
									<span style='color: #20a0ff;font-size: 2rem;'>{{watch_times}}</span>人/次
								</el-row>
							</el-col>
						</el-row>
					</el-col>
				</el-tab-pane>
			</el-tabs>
		</el-row>
		<el-row class='livecontent' style='margin-top: 2rem;'>
			<el-tabs>
				<el-tab-pane label='地域分布'>
					<el-col :span='24'>
						<el-row>
							<el-col>
								<h3 style='text-align: center;'>观看人次TOP10</h3>
								<h3 v-if='h3if' style='display: flex;justify-content: center;margin-top: 2rem;margin-bottom: 2rem;'>很抱歉{{nm}}没有人观看哦</h3>
							</el-col>
						</el-row>
						<el-row>
							<el-col style='display: flex;justify-content: center;'>
								<div v-show='echartif' class='echartcontainer' style='width: 40rem;height: 25rem;'>
									<div id='echartid' style='width: 40rem;height: 25rem;'>
									</div>
								</div>
							</el-col>
						</el-row>
					</el-col>
				</el-tab-pane>
			</el-tabs>
		</el-row>
	</div>
</template>

<script>
	import Echarts from 'echarts'
	import store from 'store'
	export default {
		name: 'livepeople',
		data() {
			return {
				isActive1: true,
				isActive2: false,
				isActive3: false,
				isActive4: false,
				isActive5: false,
				isActive6: false,
				// 直播时长
				live_duration: '',
				// 观看时长
				watch_Tduration: '',
				// UV
				audience_num: '',
				// PV
				watch_times: '',
				// 观看人省份
				watch_province: [],
				//对应省份的人数
				watch_num: [],
				//其实不用把这个变量定义出来 只是为了判断有没有人观看 好为计算属性true或false来判断echart的显示与否
				watch_list: [],
				//这个只是为了没有人观看时 不显示echart 显示什么时候没有人观看
				nm: '',
				activeName: 'first'
			}
		},
		computed: {
			echartif: function() {
				if(this.watch_list.length == 0) {
					return false
				} else {
					return true
				}
			},
			h3if: function() {
				if(this.echartif == false) {
					return true
				} else {
					return false
				}
			}
		},
		methods: {
			// 顶部不同时期的点击事件
			chose: function(num) {
				// 今日
				if(num == 1) {
					this.isActive1 = true
					this.isActive2 = false
					this.isActive3 = false
					this.isActive4 = false
					this.isActive5 = false
					this.isActive6 = false
					this.getRoomTotal(1)
				} else if(num == 2) {
					// 昨日
					this.isActive1 = false
					this.isActive2 = true
					this.isActive3 = false
					this.isActive4 = false
					this.isActive5 = false
					this.isActive6 = false
					this.getRoomTotal(2)
				} else if(num == 3) {
					// 本周
					this.isActive1 = false
					this.isActive2 = false
					this.isActive3 = true
					this.isActive4 = false
					this.isActive5 = false
					this.isActive6 = false
					this.getRoomTotal(3)
				} else if(num == 4) {
					// 上周
					this.isActive1 = false
					this.isActive2 = false
					this.isActive3 = false
					this.isActive4 = true
					this.isActive5 = false
					this.isActive6 = false
					this.getRoomTotal(4)
				} else if(num == 5) {
					// 本月
					this.isActive1 = false
					this.isActive2 = false
					this.isActive3 = false
					this.isActive4 = false
					this.isActive5 = true
					this.isActive6 = false
					this.getRoomTotal(5)
				} else {
					// 上月
					this.isActive1 = false
					this.isActive2 = false
					this.isActive3 = false
					this.isActive4 = false
					this.isActive5 = false
					this.isActive6 = true
					this.getRoomTotal(6)
				}
			},
			// 访问接口拿取观众列表统计
			getRoomTotal: function(time) {
				this.$http.get('/analyzes/audience/room/' + this.$route.params.id + '?access-token=' + localStorage.getItem('accessToken')).then((response) => {
					console.log('success')
					console.log(response.body)
					// 默认是今天的数据分析
					this.$http.post('/analyzes/data/room/' + this.$route.params.id, { time_slot: time }).then((response) => {
						console.log('success')
						console.log(response.body)
						// 观看总时长
						this.watch_Tduration = response.body.data.situation.watch_duration
						// 直播时长
						this.live_duration = response.body.data.situation.live_duration
						// UV
						this.audience_num = response.body.data.situation.audience_num
						// PV
						this.watch_times = response.body.data.situation.watch_times
						//观看数据
						this.watch_list = response.body.data.area
						if(time == 1) {
							this.nm = '今日'
						} else if(time == 2) {
							this.nm = '昨日'
						} else if(time == 3) {
							this.nm = '本周'
						} else if(time == 4) {
							this.nm = '上周'
						} else if(time == 5) {
							this.nm = '本月'
						} else {
							this.nm = '上月'
						}
						//如果没有人则不显示echart
						if(this.watch_list.length == 0) {

						} else {
							this.watch_province = []
							this.watch_num = []
							var maxnum = 0
							//这里先遍历拿到各个省份
							//然后定义一个变量拿到总人数
							for(var a = 0; a < this.watch_list.length; a++) {
								this.watch_province[a] = this.watch_list[a].province
								maxnum = maxnum + parseInt(this.watch_list[a].audience_num)
								console.log(maxnum)
							}
							//这里再遍历一次 用每个省份的人数除以总人数得到百分比
							for(var a = 0; a < this.watch_list.length; a++) {
								this.watch_num[a] = parseInt(this.watch_list[a].audience_num / maxnum * 100)
								console.log(this.watch_num[a])
							}
							var mychart = Echarts.init(document.getElementById('echartid'))
							mychart.showLoading()
							var that = this
							mychart.setOption({
								title: {
									text: '',
									subtext: ''
								},
								tooltip: {
									trigger: 'axis',
									axisPointer: {
										type: 'shadow'
									},
									formatter: "{a} <br/>{b} : {c}%"
								},
								legend: {
									data: ['2016年']
								},
								grid: {
									left: '3%',
									right: '4%',
									bottom: '3%',
									containLabel: true
								},
								xAxis: {
									type: 'value',
									boundaryGap: [0, 0.01],
									'axisLabel': {
										'interval': 0,
										formatter: '{value}%'
									}
								},
								yAxis: {
									type: 'category',
									data: this.watch_province
								},
								series: [{
									name: this.nm,
									type: 'bar',
									//在这里显示百分比
									data: this.watch_num,
									//然后在右部显示具体的人数
									label: {
										normal: {
											show: true,
											position: 'right',
											formatter: function(obj) {
												return Math.ceil(obj.value / 100 * maxnum) + '人'
											}
										}
									},
									itemStyle: {
										normal: {
											color: function(params) {
												return '#20a0ff'
											}
										}
									}
								}],
								textStyle: {
									color: 'black'
								}
							})
							mychart.hideLoading()
						}

					}, (response) => {
						console.log('error')
						console.log(response.body)
					})
				}, (response) => {
					console.log('error')
					console.log(response.body)
				})
			},

		},
		create() {

		},
		mounted() {
			// 拿到该直播间的统计 并实例化Echart
			this.getRoomTotal(1)
		}
	}
</script>

<style lang='less'>
	.active {
		color: #20a0ff;
		border-color: #20a0ff;
	}
	
	.livepeople {
		h6 {
			border-left: 1px solid black;
			padding: 0.2rem;
			border-top: 1px solid black;
			border-bottom: 1px solid black;
			cursor: pointer;
		}
		.livecontent {
			h2.contentitle {
				background: gainsboro;
				padding-left: 1rem;
				padding-top: 0.5rem;
				padding-bottom: 0.5rem;
				border-radius: 0.5rem;
			}
		}
		.gaiitems{
			margin-top: 2rem;
			text-align: center;
			border: 1px solid #20a0ff;
			border-radius: 6px;
			padding-bottom: 2rem;
			.el-row{
				margin-top: 2rem;
			}
		}
	}
</style>