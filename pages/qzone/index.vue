<template>
	<view class="page" :style="{height:`${sysHeight}px`,width:`${sysWidth}px`}">
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
				>
					<view class="cardBox"
						@touchend="touchend" 
						@touchmove="touchMove"
						@touchstart="touchStart"
						:animation="animationData[index]"
						:style="{transform:index<number?`rotate(${rotate*index}deg) scale(${ 1-(1-scale.x)*index },${ 1-(1-scale.y)*index }) skew(${skew.x*index}deg, ${skew.y*index}deg) translate(${translate.x*index}px, ${translate.y*index}px)`:`rotate(${rotate*(number-1)}deg) scale(${ 1-(1-scale.x)*(number-1) },${ 1-(1-scale.y)*(number-1) }) skew(${skew.x*(number-1)}deg, ${skew.y*(number-1)}deg) translate(${translate.x*(number-1)}px, ${translate.y*(number-1)}px)`,opacity:index<number?`${ 1-(1-opacity)*index }`:`${ 1-(1-opacity)*(number-1) }`}"
						
					>
					{{item.id}}
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
				this.number = 4 //card 4
				
				//设置第2张卡片transform opacity
				
				this.translate = { x:0,y:50 } //y下移10px
				this.scale = { x:0.9,y:1 }, //x 缩小0.9
				this.rotate = 10 //旋转deg 
				this.skew = { x:20,y:0 }, //倾斜px
				this.opacity = 0.95  //透明度，参数范围 0~1
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
	.move-area{
		position: absolute;
	}
	.move-view{
		width: 300rpx;
		height: 400rpx;
		left: 50%;
		top: 50%;
		margin-left: -150rpx;
		margin-top: -500rpx;
	}
	.cardBox{
		
		background-color: #FFFFFF;
		position:absolute;
		width: 300rpx;
		height: 400rpx;
		border-radius: 20rpx;
		border: 2px solid #e9e7ef;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 20px;
		
	}
	
	
	
</style>
