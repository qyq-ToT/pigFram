<template>
	<view class="page-body">
		<!-- //养猪场 -->
		<movable-area class="pigFarm">
			<movable-view class="iconRow" direction="all" inertia damping=5 friction=12 animation @touchstart="touchstart(index)"
			v-for="(item,index) in pigList" :x="item.x" :y="item.y" @touchend="touchend" @change="onChange">
				<image class="pigIcon" :src="item.src"></image>
			</movable-view>
			
			<movable-view class="smImgRow" direction="all" inertia damping=5 friction=12 animation
			v-for="(item2,index2) in smPigList" :x="item2.x" :y="item2.y">
				<image @click="smPigMove(index2)" class="smPigIcon" :src="item2.src"></image>
			</movable-view>
			
		</movable-area>
		
		<view @click="livePig()" class="livePig">100</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				touchElIndex:"",
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
						x:51,
						y:107,
						src:"../../static/ic_cash_pig.png",
						grade:1
					},
					{
						x:205,
						y:96,
						src:"../../static/ic_cash_pig.png",
						grade:1
					}
				]
			}
		},
		onLoad() {
			// setTimeout(()=>{this.smPigMove()},5000)
			uni.showTabBarRedDot({
				index:2
			})
		},
		methods: {
			smPigMove(i){
				// for(let i = 0;i<this.smPigList.length;i++){
				// 	this.smPigList[i].x = 320
				// 	this.smPigList[i].y = 0
				// 	setTimeout(()=>{this.smPigList.splice(i,1)},3000)
				// }
				this.smPigList[i].x = 320
				this.smPigList[i].y = 0
				setTimeout(()=>{this.smPigList.splice(i,1)},3000)
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
				// console.log(x)
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
				// console.log(src)
				this.pigList[i].src = src
				this.pigList[i].grade = grade+1
				this.pigList.splice(skip,1)
				console.log(this.pigList)
			},
			touchend:function(e){},
			// 点击生成一头猪
			livePig(){
				let px = 77;
				let py = 24
				let pig = {
					x:px,
					y:py,
					src:"../../static/ic_pig_1.png",
					grade:1
				}
				let smPig = {
					x:px-15,
					y:py+30,
					src:"../../static/ic_cash_pig",
					grade:1
				}
				this.pigList.push(pig)
				this.smPigList.push(smPig)
				console.log(this.pigList)
				console.log(this.smPigList)
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
			height: 80%;
			border: 1px solid #007AFF;
			.iconRow{
				width: 160rpx;
				height: 120rpx;
				.pigIcon{
					width: 100%;
					height: 100%;
				}
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
				.smPigIcon{
					width: 100%;
					height: 100%;
				}
			}
		}
	}
</style>