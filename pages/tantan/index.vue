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
					:animation="animationData[count]"
					:style="{transform:`rotate(${rotate*count}deg) scale(${ 1-(1-scale.x)*count },${ 1-(1-scale.y)*count }) skew(${skew.x*count}deg, ${skew.y*count}deg) translate(${translate.x*count}px, ${translate.y*count}px)`,zIndex:9999-count,opacity:`${ 1-(1-opacity)*count }`}"
				>
				
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
				dataList:[

				]
			}
		},
		async mounted() {
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
			getData(){
				let promise = new Promise((resolve,reject)=>{
					uni.request({
						url: 'https://www.apiopen.top/meituApi?page=3',
						success: (res) => {
							let dataGroup = []
							for (let item of res.data.result) {
								dataGroup.push({
									url:item.data.url
								})
							}
							this.dataList = [...this.dataList,...dataGroup]
							resolve()
						}
					})
				}) 
				return promise
			},
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
			background-color: #FFFFFF;
			position:absolute;
			width: 600rpx;
			height: 900rpx;
			border-radius: 20rpx;
			border: 2px solid #e9e7ef;
		}
	}
</style>
