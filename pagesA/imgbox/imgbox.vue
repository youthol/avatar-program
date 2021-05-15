<template>
	<view class="page-box">
		<view class="img-box" v-for="(items, index) in imgList" :key="index">
			<navigator url="../../pagesB/newimg/newimg"><image :src="items.src" mode="aspectFit" show-menu-by-longpress=true style="width: 300rpx; height: 200rpx;" @click="sendimg(items.id)"></image></navigator>
			<text class="img-name">{{items.name}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgid: '',
				tempFilePath: '',
				imgList: [
					{
						id: 1,
						src: require('../../static/1.png'),
						name: '山理工欢迎你'
					},
					{
						id: 2,
						src: require('../../static/2.png'),
						name: '阿线敬礼'
					},
					{
						id: 3,
						src: require('../../static/3.png'),
						name: '校徽经典款'
					},
					{
						id: 4,
						src: require('../../static/4.png'),
						name: '稷下之约'
					},
					{
						id: 5,
						src: require('../../static/5.png'),
						name: '花已灿'
					},
					{
						id: 6,
						src: require('../../static/6.png'),
						name: '一枚鸿远'
					},
					{
						id: 7,
						src: require('../../static/7.png'),
						name: '夏之绽放'
					},
					{
						id: 8,
						src: require('../../static/8.png'),
						name: '我爱山理工'
					},
				]
			}
		},
		onLoad() {
			var _this = this
			uni.compressImage({
			  src: '../../static/8.png',
			  quality: 80,
			  success: res => {
			    console.log(res.tempFilePath)
				_this.tempFilePath = res.tempFilePath
			  }
			})
		},
		onHide() {
			uni.$emit('sendid',{msg:this.imgid})
			console.log('销毁执行')
		},
		methods: {
			sendimg (datas) {
				this.imgid = datas
			}
		}
	}
</script>

<style>
	.page-box {
		margin-top: 100rpx;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
		align-items: space-around;
	}
	.img-box {
		width: 300rpx;
		height: 400rpx;
		overflow: hidden;
		margin: 20rpx 10rpx 20rpx 10rpx;
	}
	.img-name {
		margin-top: 20rpx;
		display: flex;
		justify-content: center;
		font-size: 25rpx;
	}
</style>
