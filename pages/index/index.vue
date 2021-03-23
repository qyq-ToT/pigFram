<template>
	<view class="page-body">
		<!-- //养猪场 -->
		<movable-area class="pigFarm">
			<movable-view damping=5 friction=12 animation @touchstart="touchstart(index)" @touchend="touchend" @change="onChange" v-for="(item,index) in pigList" class="iconRow" :x="item.x" :y="item.y" direction="all" inertia>
				<image class="pigIcon" :src="item.src"></image>
			</movable-view>
		</movable-area>
		<movable-area class="smPigFarm">
			<movable-view class="smImgRow" direction="all" inertia damping=5 friction=12 animation>
				<image x="100" y="20" src="../../static/ic_cash_pig.png"></image>
			</movable-view>
		</movable-area>
		<view @click="livePig()" class="livePig">100</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// x: 0,
				// y: 0,
				// scale: 2,
				touchElIndex:"",
				// old: {
				// 	x: 0,
				// 	y: 0,
				// 	scale: 2
				// },
				pigList:[
					{
						x:66,
						y:77,
						src:"../../static/ic_pig_1.png",
						grade:1
					},
					{
						x:220,
						y:66,
						src:"../../static/ic_pig_1.png",
						grade:1
					}
				],
				smPigList:[
					{
						src:"../../static/ic_cash_pig.png"
					}
				]
			}
		},
		onLoad() {
			this.pigSport();
		},
		methods: {
			tap: function(e) {
				// 解决view层不同步的问题
				this.x = this.old.x
				this.y = this.old.y
				this.$nextTick(function() {
					this.x = 30
					this.y = 30
				})
				console.log(111)
			},
			tap2() {
				// 解决view层不同步的问题
				this.scale = this.old.scale
				this.scale = this.old.scale
				this.$nextTick(function() {
					this.scale = 3
				})
			},
			//当拖拽动作开始是将被操作的元素的索引存进data里面
			touchstart(index){
				this.touchElIndex = index;
			},
			onChange: function(e) {
				let x = e.detail.x
				let y = e.detail.y
				// this.old.x = x
				// this.old.y = y
				// console.log(this.touchElIndex)
				//定义循环要跳过的条件
				console.log(x)
				let skip = parseInt(this.touchElIndex);
				this.pigList[skip].x = x
				this.pigList[skip].y = y
				//循环判断移动中有没有经过那个猪的范围。
				for(let i = 0;i<this.pigList.length;i++){
					if(i == skip){
						continue;//若i=1，则进行下一次循环
					}
					if(x>this.pigList[i].x-10&&x<this.pigList[i].x+10){
						// console.log(x)
						if(y>this.pigList[i].y-10&&y<this.pigList[i].y+10){
							// console.log(x,y)
							console.log("猪碰到了一起");
							//判断两个猪的等级是否一致
							console.log(this.pigList[i].grade==this.pigList[skip].grade)
							if(this.pigList[i].grade == this.pigList[skip].grade){
								//当两个猪接近重叠的时候调用合并升级方法 参数（升级的猪的索引，删除的猪的索引
								this.mergeupGrade(i,skip)
							}
						}
					}
				}
			},
			// 猪升级的方法 参数（升级的猪的索引，删除的猪的索引
			mergeupGrade(i,skip){
				console.log("第"+i+"个猪要被升级");
				let grade = this.pigList[i].grade
				let src = "../../static/ic_pig_"+ (grade+1) +".png"
				console.log(src)
				this.pigList[i].src = src
				this.pigList[i].grade = grade+1
				this.pigList.splice(skip,1)
				console.log(this.pigList)
			},
			touchend:function(e){},
			// 点击生成一头猪
			livePig(){
				console.log("生成一头猪")
				let pig = {
					x:77,
					y:24,
					src:"../../static/ic_pig_1.png",
					grade:1
				}
				this.pigList.push(pig)
				console.log(this.pigList)
				// this.$forceUpdate() 
			},
			// 使猪运动的方法
			pigSport(){
				// this.$nextTick(function() {
				//     for(let i = 0;i<this.pigList.length;i++){
				//     	// let rnumx = Math.floor(Math.random()*50)-20;
				//     	// let rnumy = Math.floor(Math.random()*30)-20;
				//     	console.log(1)
				//     	// this.pigList[i].x += Math.floor(Math.random()*100)-50;
				//     	// this.pigList[i].y += Math.floor(Math.random()*10)-5;
				//     	this.pigList[i].x += 30;
				//     	this.pigList[i].y += 30;
				// 		this.onChange()
				//     }
				// })
				
			},
			//小猪运动的方法
			smPigSport(){
				
			}
		}
	}
</script>

<style lang="scss">
	.page-body{
		width: 100%;
		height: 100vh;
		background: #FFFFFF;
		position: relative;
		.pigFarm{
			width: 100%;
			height: 70%;
			border: 1px solid #007AFF;
			.iconRow{
				width: 160rpx;
				height: 120rpx;
				.pigIcon{
					width: 100%;
					height: 100%;
				}
				.smImgRow{
					width: 32px;
					height: 24px;
					image{
						width: 100%;
						height: 100%;
					}
				}
			}
			
		}
		.smPigFarm{
			width: 100%;
			height: 70%;
			position: absolute;
			left: 0;
			right: 0;
			top: 0;
			bottom: 0;
			.smImgRow{
				width: 36px;
				height: 24px;
				image{
					width: 100%;
					height: 100%;
				}
			}
		}
	}
</style>