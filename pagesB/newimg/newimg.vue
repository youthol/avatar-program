<template>
	<view class="new-img">
		<image :src="src" mode="aspectFit" show-menu-by-longpress=true  style="width: 400rpx; height: 400rpx;"  id="imageone" v-if="!showCanvas"></image>
		<canvas style="width: 400rpx; height: 400rpx;"  canvas-id="myCanvas" id="myCanvas" v-show="showCanvas"></canvas>
		<button size="mini" @click="getimg">授权头像</button>
		<button size="mini" style="background-color: red;" @click="save">生成头像</button>
	</view>
</template>

<script>
	var imgsrc = ''
	var avatar = ''
	var rpxsize = ''
	uni.$on('sendid',function(data){
	  this.id = data.msg
	  imgsrc = '../../static/' + this.id + '.png'
	  console.log(imgsrc)
	})
	export default {
		data () {
			return {
				id: '',
				src: imgsrc,
				showCanvas: false,
				screenWidth: ''
			}
		},
		onLoad () {
			var _this = this
			uni.getSystemInfo({  //获取屏幕尺寸
			    success: function(res) {
			        //console.log(res.screenWidth)
					_this.screenWidth = res.screenWidth
					console.log(_this.screenWidth)
					console.log(res.screenHeight)
					rpxsize = _this.screenWidth / 750
			    },
			})
		},
		methods: {
			getimg () {
				// 微信授权登录
				var _this = this
				var avatar = ''
				// console.log('1')
				uni.getUserProfile({
				  // provider: 'weixin',
				  desc: '仅获取头像信息',
				  success: function (loginRes) {
					console.log(loginRes.userInfo)
						// avatar = loginRes.userInfo.avatarUrl
						avatar = loginRes.userInfo.avatarUrl.replace("132", "0").replace('https://thirdwx.qlogo.cn', 'https://wx.qlogo.cn')
						console.log(avatar)
						uni.downloadFile({  // 下载网络图片生成本地地址
						    url: avatar, 
							timeout: 60000,
						    success: (res) => {
						        if (res.statusCode === 200) {
						            console.log(res)
									// 绘制画布
									var ctx = uni.createCanvasContext('myCanvas')
									ctx.drawImage(res.tempFilePath, 0, 0, 400*rpxsize, 400*rpxsize)
									ctx.drawImage(imgsrc, 0, 0, 400*rpxsize, 400*rpxsize)
									ctx.draw()
									_this.showCanvas = true
					            }
					        },
							fail:(res) => {
								 // console.log('downloadFile fail, err is:', err)
								uni.showToast({
								    title: '获取头像失败',
								    duration: 2000
								})
							}
					    })
		          }
		        })
		    },
			save () {
			  //图片绘制完成
			  uni.canvasToTempFilePath({
			    x: 0,
			    y: 0,
			    width: 400,
			    height: 400,
			    destWidth: 400,
			    destHeight: 400,
				fileType: 'jpg',
				quality: 1,
			    canvasId: 'myCanvas',
			    success: function(res) {
			      // 在H5平台下，tempFilePath 为 base64
			      console.log(res.tempFilePath)
				  // 保存图片
				  uni.saveImageToPhotosAlbum({
				      filePath: res.tempFilePath,//canvasToTempFilePath返回的tempFilePath
				      success: (res) => {
				          console.log(res)
						  uni.showToast({
						      title: '65周年生日快乐',
						      duration: 2000
						  })
				      },
				      fail: (err) => {}
				  })
			    } 
			  })
			}
		},
		watch: {
			src (newvalue, old) {
				this.src = require(imgsrc)
			}
		}
	}
</script>

<style>
	.new-img {
		margin: 100rpx 0rpx 30rpx 0rpx;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-content: space-around;
	}
	
	.new-img image, canvas {
		margin: 50rpx auto;
	}
	
	.new-img button {
		margin-bottom: 40rpx;
	}
</style>
