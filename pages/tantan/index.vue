<template>
	<view class="page" :style="{height:`${sysHeight}px`}">
		<movable-area class="move-area" :style="{height:`${3*sysHeight}px`,width:`${3*sysWidth}px`,top:`${-sysHeight}px`,left:`${-sysWidth}px` }">
				<movable-view
					id="move"
					class="move-view"
					v-for="(item,index) in dataList"
					:key="item._id"
					:style="{zIndex:`${9999-index}`}"
					direction="all"
					:x="item.moveX"
					:y="item.moveY"
					out-of-bounds
					v-if="index<=number"
					:disabled="index!=0"
					:animation="item.animation"
					@tap="tapCard(item)"
				>
					<view class="cardBox"
						@touchend="touchend" 
						@touchmove="touchMove"
						@touchstart="touchStart"
						:animation="animationData[index]"
						:style="{transform:index<number?`rotate(${rotate*index}deg) scale(${ 1-(1-scale.x)*index },${ 1-(1-scale.y)*index }) skew(${skew.x*index}deg, ${skew.y*index}deg) translate(${translate.x*index}px, ${translate.y*index}px)`:`rotate(${rotate*(number-1)}deg) scale(${ 1-(1-scale.x)*(number-1) },${ 1-(1-scale.y)*(number-1) }) skew(${skew.x*(number-1)}deg, ${skew.y*(number-1)}deg) translate(${translate.x*(number-1)}px, ${translate.y*(number-1)}px)`,opacity:index<number?`${ 1-(1-opacity)*index }`:`${ 1-(1-opacity)*(number-1) }`}"
						
					>
						<view class="top">
							<view class="img-top">
								<view class="img-left">
									<view class="img-number"><view class="iconfont icon-tupian1 tupian"></view> {{item.number}}</view>
									<view class="img-zou"><view class="iconfont icon-genwozou genwozou"></view> 擦肩而过1次</view>
								</view>
								<view class="img-right">
									<view class="iconfont icon-v"></view>
								</view>
							</view>
							<view class="love" :animation="loveAnimation[index]">
								<view class="iconfont icon-xinaixin" :style="{fontSize:'60rpx'}"></view>
							</view>
							<view class="loathe" :animation="loatheAnimation[index]">
								<view class="iconfont icon-chacha1" :style="{fontSize:'60rpx'}"></view>
							</view>
							<image class="img" :src="item.src" v-if="index<number"></image>
						</view>
						
						<view class="bottom">
							<view class="star">
								<view class="iconfont icon-wujiaoxing1" :style="{fontSize:'60rpx'}"></view>
							</view>
							<view>{{item.name}}</view>
							<view class="labelBox">
								<view class="label" :style="{backgroundColor:item.sex==0?'#7BD8FF':'#F3C9F5'}">
									<view class="iconfont sex" :class="item.sex==0?'icon-male':'icon-xingbie-nv'" :style="{fontSize:'16px' }"></view> {{item.old}}</view>
								<view class="label" :style="{backgroundColor:'#A4C742'}">{{item.constellation}}</view>
							</view>
							<view class="address">{{item.address}}</view>
						</view>
					  </view>
				</movable-view>
		</movable-area>
	</view>
</template>

<script>
	import clCardDel from "@/components/cl-cardDel/cl-cardDel";
	export default{ 
		mixins:[clCardDel],
		data(){
			return{
				loveAnimation:{0:{}},
				loatheAnimation:{0:{}},
				loveAni:null,
				loatheAni:null
			}
		},
		onLoad() {
			
		},
		async mounted() {
			//touch移动喜欢动画
			this.loveAni = uni.createAnimation({
				duration:0
			});
			//touch移动不喜欢动画
			this.loatheAni = uni.createAnimation({
				duration:0
			});
			await this.getData()
		},
		methods:{
			//设置初始参数
			init(){
				this.number = 3 //card 3
				this.translate = { x:0,y:8 } //y下移10px
				this.scale = { x:0.95,y:1 }, //x 缩小0.9
			
				this.moveRotate = { //设置位移图片旋转角度距离  card中心点 - 指向坐标
					x:0,
					y:uni.getSystemInfoSync().screenHeight ,
				}
			},
			//获取数据
			getData(){
				let promise = new Promise((resolve,reject)=>{
					let dataGroup = []
					for (var i = 1; i < 6; i++) {
						dataGroup.push({
							src:`../../static/${i}.jpg`,
							sex:Math.round(Math.random()),
							address:'杭州(100km)',
							name:'可爱的小姐姐',
							constellation:'双鱼座',
							number:10,
							old:18
						})
					}
					this.dataList = [...this.dataList,...dataGroup]
					resolve()
				}) 
				return promise
			},
			//触摸中判断
			moveJudge(x,y,ratio){
				//touch移动喜欢动画
				this.loveAni = uni.createAnimation({
					duration:0
				});
				//touch移动不喜欢动画
				this.loatheAni = uni.createAnimation({
					duration:0
				});
				
				if(x>20){
					this.loveAni.opacity( 0.3 + 0.7*ratio ).step()
					this.loveAnimation[0] = this.loveAni.export()
					this.loatheAni.opacity( 0 ).step()
					this.loatheAnimation[0] = this.loatheAni.export()
				}else if(x<-20){
					this.loveAni.opacity( 0 ).step()
					this.loveAnimation[0] = this.loveAni.export()
					this.loatheAni.opacity( 0.3 + 0.7*ratio ).step()
					this.loatheAnimation[0] = this.loatheAni.export()
				}else{
					this.loveAni.opacity( 0.3 ).step()
					this.loveAnimation[0] = this.loveAni.export()
					this.loatheAni.opacity( 0.3 ).step()
					this.loatheAnimation[0] = this.loatheAni.export()
				}
			},
			//触摸结束判断
			endJudge(x,y){
				if(Math.abs(x)<40){
					
					this._back()
					//touch移动喜欢动画
					this.loveAni = uni.createAnimation({
						duration:200
					});
					//touch移动不喜欢动画
					this.loatheAni = uni.createAnimation({
						duration:200
					});
					this.loveAni.opacity( 0 ).step()
					this.loveAnimation[0] = this.loveAni.export()
					this.loatheAni.opacity( 0 ).step()
					this.loatheAnimation[0] = this.loatheAni.export()
				}else{
					this._del()
					this.loveAnimation[0] = this.loveAni.export()
					this.loatheAnimation[0] = this.loatheAni.export()
				}
			},
			//删除card时
			delCard(x,y){
				if(x>0){
					console.log(this.dataList[0],'喜欢')
				}else{
					console.log(this.dataList[0],'不喜欢')
				}
			},
			tapCard(item){
				console.log(item,"点击")
			},
			tapCard(item){
				console.log(item)
			}
		}
		
	}
</script>


<style lang="scss" scoped>
	@font-face {font-family: "iconfont";
	  src:  url('data:application/x-font-woff2;charset=utf-8;base64,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') format('woff2');
	}
	
	.iconfont {
	  font-family: "iconfont" !important;
	  font-size: 12px;
	  font-style: normal;
	  -webkit-font-smoothing: antialiased;
	  -moz-osx-font-smoothing: grayscale;
	}
	
	
	.icon-v:before {
	  content: "\e670";
	}
	
	.icon-male:before {
	  content: "\e600";
	}
	
	.icon-wujiaoxing1:before {
	  content: "\e602";
	}
	
	.icon-chacha1:before {
	  content: "\e646";
	}
	
	.icon-genwozou:before {
	  content: "\e61d";
	}
	
	.icon-xinaixin:before {
	  content: "\e601";
	}
	
	.icon-tupian1:before {
	  content: "\e718";
	}
	
	.icon-xingbie-nv:before {
	  content: "\e677";
	}
	
	.page{
		width: 100%;
		position: absolute;
		overflow: hidden;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.move-area{
		position: absolute;
	}
	.move-view{
		width: 600rpx;
		height: 900rpx;
		left: 50%;
		top: 50%;
		margin-left: -300rpx;
		margin-top: -500rpx;
	}
	.cardBox{
		position:relative;
		width: 600rpx;
		height: 900rpx;
		background-color: #fff;
		border-radius: 20rpx;
		border: 2px solid #e9e7ef;
		overflow: hidden;
		.top{
			width: 600rpx;
			height: 700rpx;
			.img-top{
				position: absolute;
				font-size: 12px;
				top: 20rpx;
				padding: 0 20rpx;
				display: flex;
				width: 560rpx;
				flex-direction: row;
				justify-content: space-between;
				z-index: 999;
				.img-left{
					display: flex;
					flex-direction: row;
					align-items: center;
					color: #fff;
					.img-number{
						display: flex;
						flex-direction: row;
						align-items: center;
						padding: 10rpx;
						background-color: rgba(0,0,0,0.2);
						border-radius: 10rpx;
						margin-right: 20rpx;
						.tupian{
							margin-right: 5rpx;
						}
					}
					.img-zou{
						display: flex;
						flex-direction: row;
						align-items: center;
						padding: 10rpx;
						background-color: rgba(0,0,0,0.4);
						border-radius: 10rpx;
						.genwozou{
							margin-right: 20rpx;
						}
					}
				}
				.img-right{
					width: 50rpx;
					height: 50rpx;
					border-radius: 50%;
					display: flex;
					justify-content: center;
					align-items: center;
					background-color: #54C7FC;
					color: #FFFFFF;
				}
			}
			.img{
				width: 600rpx;
				height: 700rpx;
			}
			.love{
				position: absolute;
				width: 120rpx;
				height: 120rpx;
				top: 60rpx;
				left: 40rpx;
				border-radius: 50%;
				background-color: #EA2A36;
				display: flex;
				justify-content: center;
				align-items: center;
				z-index: 999;
				color: #FFFFFF;
				opacity: 0;
			}
			.loathe{
				position: absolute;
				width: 120rpx;
				height: 120rpx;
				top: 60rpx;
				right: 40rpx;
				border-radius: 50%;
				background-color: #A09E9F;
				display: flex;
				justify-content: center;
				align-items: center;
				z-index: 999;
				color: #FFFFFF;
				opacity: 0;
			}
		}
		.bottom{
			position: relative;
			display: flex;
			padding: 10rpx 20rpx;
			flex-direction: column;
			color:#202020;
			.labelBox{
				margin-top: 10rpx;
				display: flex;
				flex-direction: row;
				.label{
					padding: 10rpx;
					font-size: 12px;
					color: #fff;
					display: flex;
					flex-direction: row;
					border-radius: 8rpx;
					margin-right: 15rpx;
				}
				.sex{
					margin-right: 5rpx;
				}
			}
			
			.address{
				margin-top: 10rpx;
				font-size: 12px;
				color: #B9B9B9;
			}
			.star{
				position: absolute;
				width: 120rpx;
				height: 120rpx;
				top: -60rpx;
				right: 50rpx;
				border-radius: 50%;
				background-image: linear-gradient(to bottom left, #87DEFF , #1FAEFF);
				display: flex;
				justify-content: center;
				align-items: center;
				
				color: #FFFFFF;
			}
		}
	}
</style>
