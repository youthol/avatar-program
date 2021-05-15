<template>
	<view class="content">
		<image src="../../static/bg.jpg" mode="aspectFit" show-menu-by-longpress=true class="bg-img"></image>
		<view class="text-area">
			<view class="big-box">
			  <navigator url="../../pagesA/imgbox/imgbox">
			  	<button size="mini" >
			        一起换头像吧
			      </button>
			  </navigator>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '你好',
				screenHeight: '',
				screenWidth: ''
			}
		},
		onLoad () {
			var _this = this
			uni.getSystemInfo({  //获取屏幕尺寸
			    success: function(res) {
			        //console.log(res.screenWidth)
					_this.screenWidth = res.screenWidth
					_this.screenHeight = res.screenHeight
					console.log(_this.screenWidth)
					console.log(res.screenHeight)
			    },
			})
		},
		methods: {
			
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		position: relative;
	}
	
	.big-box {
		position: absolute;
		top: 700rpx;
		display: flex;
		justify-content: center;
	}
	
	.bg-img {
		width: 700rpx;
		height: 900rpx;
		margin: 100rpx auto;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}
	
	button {
		background-color: rgb(159,58,62);
		color: white;
		font-size: 20rpx;
	}
</style>