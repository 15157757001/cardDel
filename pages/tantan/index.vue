<template>
	<view class="page">
		<view class="mask">
			<view class="cardBox">
				<view
					class="card" 
					:key="item._id"
					@touchend="touchend" 
					v-for="(item,index) in dataList"
					@touchmove="touchMove"
					@touchstart="touchStart"
					:animation="animationData[index]"
					:style="{transform:index<number?`rotate(${rotate*index}deg) scale(${ 1-(1-scale.x)*index },${ 1-(1-scale.y)*index }) skew(${skew.x*index}deg, ${skew.y*index}deg) translate(${translate.x*index}px, ${translate.y*index}px)`:`rotate(${rotate*(number-1)}deg) scale(${ 1-(1-scale.x)*(number-1) },${ 1-(1-scale.y)*(number-1) }) skew(${skew.x*(number-1)}deg, ${skew.y*(number-1)}deg) translate(${translate.x*(number-1)}px, ${translate.y*(number-1)}px)`,zIndex:index<number?`${9999-index}`:`${9999-(number-1)}`,opacity:index<number?`${ 1-(1-opacity)*index }`:`${ 1-(1-opacity)*(number-1) }`}"
				>
					<image class="img" :src="item.src"></image>
					<view class="bottom">
						<view>{{item.name}}</view>
						<view class="labelBox">
							<view class="label" :style="{backgroundColor:item.sex==0?'#7BD8FF':'#F3C9F5'}">
								<view class="iconfont sex" :class="item.sex==0?'icon-male':'icon-xingbie-nv'"></view> {{item.old}}</view>
							<view class="label" :style="{backgroundColor:'#A4C742'}">{{item.constellation}}</view>
						</view>
						<view class="address">{{item.address}}</view>
					</view>
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
							for (let item of res.data.data) {
								dataGroup.push({
									src:item.url,
									sex:Math.round(Math.random()),
									address:'杭州(100km)',
									name:'可爱的小姐姐',
									constellation:'双鱼座',
									old:18
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
	@font-face {font-family: "iconfont";
	  src: url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAAXMAAsAAAAACrAAAAV/AAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVgCDVgqIAIZjATYCJAMcCxAABCAFhG0HZRs4CVGUT06C7CO1U42oyeSP1wi+3++357775IsDDZJoYugeNTGERPJGYqgkWmC6lUYoXgJDv6l/hEqW1IXOpOKKPDGaAKWWjlI1n6hEnqh9rX/cO/2r8yycDyiXuQZ1YRTHAQU2xsJlUZqWUKKcgng2A48JPEygbc5x0H54LAqEMkUaKLpgGRIIQzo5hxwaZZWyMB+COoG6JjUJHgGe7O/HH9gIIZCUBVCOOjkPk4LAT/BZAFfuVoK3ihpQ1Z4F3DIKrAMycZ9quwO7sXWw9uc3ag4AXY0kf0LfGn4r+zbv24LdXeAThERKInT11T94hFKWFCqiApB1adJZvOIT6BrxEUCjhNwQGhlyGTQS5HnQKCAvgNyq4vXwPgbEJgDxTkjrsqReXMjSnL6amt0J7QX0LA2M9XQPaGUYJ6WAU3LfS/JKhfL6LaoA8VVVldL/itIdSkrB316UNytfKKtUL1UtiueKSrkmUYTuwQrxkvFoHF937z93GEVPyZ0wdPLIkLjFg8DmxHNX9+HEBor2Amx01qTZBO83QmdFzf39w5ZEVShz2XRgwZQY0Nj6reeOYH3G2NxQScvDm1U3HohuyZEpBaM3e8lMY+jyAf2Fq5aaw1ek/Qebe418e2dNKrI9z6yRVL329vp9c8Zvlyi/RJWfo/L3Mepyiw+j+gh/TVrmIlqpJExVKiwuQaXAFAqUk8vRyr6WAWhGFWFWVvXjKIpRGKFMw1PReFwmgmZUKkKpxKmNFArMCIiaFyuXxGJDSy0rlpDYsFQt07mcdA42VSqxwUL6k/E7iYdB41GGP5aWlMQc5TVrI9QUnMwUgsTDd8Y/+W1pqeWPSxFWiaAUWkckUgoirBNlBb8RwIKivGaGKQrY7+nzx2+zTldcSi998N4zZwt25UXArlnYF6UT6l+ERfxpaSw+7eKop7Hu+oF/eoof1+/8HKjXfCFcKH6h539SfFORcDL1Umyjjkbw3mATjV+E4RXT1nGHj3Z/EMS5ZW2/9mpST7PiRjh5y/XHMuB++8SuXBgoLLfTDwrR14fAbS+fMdpO2swzPNf25yuI6Dv62wefn1+u64QqtaMLfUqbrWyvlV2z1QKPIPjZkiG11TPeynxLS50hqTTIQJLe361H2Pd2SQOXsB7b3R3nry33dCSmgPfjU+ySYuUwGmUhQiPBliQJCWx98qwkCCJwENQM6Q0J9YOD9Z3r6w7nZ5ahzmcJuU9MDhpq8vyju2tJ/099D2kVJ7y2fgKcuVdPwIx+bbeAFnDTQ0UClW26BZyAju3B6MNEvfHS4SXj+grB8tFlQUWorImGYtKpRIhm001QjN8dvAPg/xKMwKE27TL8AA5Jm1CrgN/ZVSor1HrgGgD+r+UpAh/pQdrVxhfv6vr+IxS/5cEPhauVXVOe91zKffgPsCZ35SCvRF+hoq+zXKOjD5ySvLa58NeOdBRJaKtNPLiusU/bU0yXLiQ0pn1IarNQaMyjmbQOpY5NqDS2oG1NxPKOCQ4s8h7AqjoCYWgDkr4XKAw9RjPpEyjN+AUqw4Ch7TQMN+xYDFYUwlEkT0mR1CKElciyac3Ii5ziZIrJyyS5oNCm4hSXywYRJ3vHaDKCyqa4Nqbk5jPOPE8jNCfLQsLJcVRmpgzJ4WTplIS3T+P5HG8HBzrtQfYSWRYQ2cOhkHgUKUSqIgiWhEw22mzNE8U+n4zCyJOJxOXU/bDiKJxcbOcIJ/YcS6ARsuxSdcdSmSsfwxlPRWgImiuUyYIIpyyUTDUmg8hJ75eOIsGzl1YjksObA9WILiuz71+RdZzboE3Zvg4BBENqkDqkAWlCQpAvyUxKQ5JGatuICvLSWVJWyGYzGnxeDktmC30NyRpSUUJVKkudyM4HAAA=') format('woff2');

	}
	
	.iconfont {
	  font-family: "iconfont" !important;
	  font-size: 16px;
	  font-style: normal;
	  -webkit-font-smoothing: antialiased;
	  -moz-osx-font-smoothing: grayscale;
	}
	
	.icon-male:before {
	  content: "\e600";
	}
	
	.icon-chacha:before {
	  content: "\e66f";
	}
	
	.icon-wujiaoxing:before {
	  content: "\e675";
	}
	
	.icon-tupian:before {
	  content: "\e613";
	}
	
	.icon-xinaixin:before {
	  content: "\e601";
	}
	
	.icon-xingbie-nv:before {
	  content: "\e677";
	}
	
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
		.img{
			width: 600rpx;
			height: 700rpx;
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
				
			}
		}
	}
</style>
