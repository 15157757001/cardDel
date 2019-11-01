<template>
	<view class="page" :style="{height:`${sysHeight}px`,width:`${sysWidth}px`}">
		<movable-area class="move-area" :style="{height:`${3*sysHeight}px`,width:`${3*sysWidth}px`,top:`${-sysHeight}px`,left:`${-sysWidth}px` }">
				<movable-view
					id="move"
					class="move-view"
					v-for="(item,index) in dataList"
					:key="item._id"
					:style="{zIndex:`${99999-item._id}`}"
					direction="all"
					:x="item.moveX"
					:y="item.moveY"
					out-of-bounds
					v-if="index<=number"
					:disabled="index!=0"
					:animation="item.animation"
					@touchend="touchend"
					@touchmove="touchMove"
					@touchstart="touchStart"
				>
					<view class="cardBox"
						:animation="animationData[index]"
						:style="{transform:index<number?`rotate(${rotate*index}deg) scale(${ 1-(1-scale.x)*index },${ 1-(1-scale.y)*index }) skew(${skew.x*index}deg, ${skew.y*index}deg) translate(${translate.x*index}px, ${translate.y*index}px)`:`rotate(${rotate*(number-1)}deg) scale(${ 1-(1-scale.x)*(number-1) },${ 1-(1-scale.y)*(number-1) }) skew(${skew.x*(number-1)}deg, ${skew.y*(number-1)}deg) translate(${translate.x*(number-1)}px, ${translate.y*(number-1)}px)`,opacity:index<number?`${ 1-(1-opacity)*index }`:`${ 1-(1-opacity)*(number-1) }`}"
						
					>
						<image class="img" :src="item.src"></image>
						<view class="content">{{item.name}}</view>
						<view class="right">
							<view class="check" @tap="tapCard(item)">查看</view>
						</view>
						<view class="right-del" @tap="tapDelCard">x</view>
						<view class="right-circle"></view>
					</view>
			</movable-view>
			<movable-view id="move" class="move-view" :style="{zIndex:`0`}" disabled>
				<view class="cardBox" :style="{transform:`scale(0.95,0.95)`}">
					
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
				
			}
		},
		methods:{
			//设置初始参数
			init(){
				this.number = 2 //card 2
				//设置第2张卡片transform opacity
				this.rotate = 5 //旋转deg 
				this.scale = { x:0.95,y:0.95 } //x 缩小0.9
			},
			//设置获取数据
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
			tapCard(item){
				console.log(item,"点击")
			},
			tapDelCard(){
				this._del()
			}
		}
		
	}
</script>

<style lang="scss" scoped>
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
		height: 200rpx;
		left: 50%;
		top: 50%;
		margin-left: -300rpx;
		margin-top: -500rpx;
	}
	.cardBox{
		background-color: #FFFFFF;
		position:absolute;
		width: 600rpx;
		height: 200rpx;
		border-radius: 20rpx;
		border: 1px solid #F8F8F8;
		box-shadow: 0upx 0upx 25upx rgba(0,0,0,0.1);
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 12px;
		overflow: hidden;
		padding:0 20upx;
		.img{
			height: 140rpx;
			width: 140rpx;
			border-radius: 50%;
		}
		.right-circle{
			position:absolute;
			width: 100upx;
			height: 100upx;
			border-radius: 50%;
			background-color: #FCF3CC;
			right:30upx;
			bottom: -60upx;
		}
		.check{
			width: 110upx;
			height: 70upx;
			border-radius: 35rpx;
			background-color:#00CAFC;
			color: #fff;
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 14px;
		}
		.right-del{
			color: #A8A8A8;
			position:absolute;
			font-size: 15px;
			right:20upx;
			top: 10upx;
		}
	}
	
	
	
</style>
