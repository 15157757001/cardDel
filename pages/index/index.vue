<template>
	<view class="page">
		<view class="mask">
			<view class="cardBox">
				<view 
					class="card" 
					:key="count"
					@touchend="touchend" 
					v-for="count in list"
					@touchmove="touchMove"
					@touchstart="touchStart"
					:animation="animationData[count-currentIndex]"
					:style="{transform:count<number?`rotate(${rotate*count}deg) scale(${ 1-(1-scale.x)*count },${ 1-(1-scale.y)*count }) skew(${skew.x*count}deg, ${skew.y*count}deg) translate(${translate.x*count}px, ${translate.y*count}px)`:`rotate(${rotate*(number-1)}deg) scale(${ 1-(1-scale.x)*(number-1) },${ 1-(1-scale.y)*(number-1) }) skew(${skew.x*(number-1)}deg, ${skew.y*(number-1)}deg) translate(${translate.x*(number-1)}px, ${translate.y*(number-1)}px)`,zIndex:count<number?`${9999-count}`:`${9999-(number-1)}`,opacity:count<number?`${ 1-(1-opacity)*count }`:`${ 1-(1-opacity)*(number-1) }`}"
				>
				{{count}}
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import clCardDel from "@/components/clCardDel/clCardDel";
	export default{ 
		mixins:[clCardDel],
		data(){
			return{
				
			}
		},
		methods:{
			//设置初始参数
			init(){
				this.number = 4 //card 4
				
				//设置第2张卡片transform opacity
				
				this.translate = { x:0,y:10 } //y下移10px
				this.scale = { x:0.9,y:1 }, //x 缩小0.9
				this.rotate = 10 //旋转deg 
				this.skew = { x:10,y:0 }, //倾斜px
				this.opacity = 0.7  //透明度，参数范围 0~1
				this.moveRotate = { //设置位移图片旋转角度距离  指向坐标 - card中心点 
					x:0,
					y:uni.getSystemInfoSync().screenHeight ,
				}
			},
			//设置获取数据
			getData(){
				let promise = new Promise((resolve,reject)=>{
					for (var i = 0; i < 10; i++) {
						this.dataList.push({id:i})
					}
					resolve()
				}) 
				return promise
			}
		}
		
	}
</script>

<style lang="scss" scoped>
	.mask{
		position: fixed;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.cardBox{
		width: 600rpx;
		height: 1000rpx;
		.card{
			position:absolute;
			width: 600rpx;
			height: 900rpx;
			background-color: #70f3ff;
			border-radius: 20rpx;
			color: #fff;
			font-size: 20px;
		}
	}
	
	
	
</style>
