<template>
	<view class="search" @click="goSearch"> 
		<input type="text" v-model="search" value="搜索">
	</view>
	<view class="uni-margin-wrap">
		<swiper class="swiper" circular :autoplay="true" :interval="2000" :duration="500" :indicator-dots="true">
			<swiper-item v-for="(item,index) in data.banners" :key="item.bannerId">
				<view class="swiper-item">
					<img :src="item.pic" alt="" @click=handlerBanner(item)>
				</view>
			</swiper-item>
		</swiper>
	</view>

	<view class="logoBox">
		<view>
			<scroll-view class="item-box" scroll-x="true">
				<view id="demo1" class="item-box-content" v-for="(item,index) in data.logoList" :key="item.id">
					<view class="box_log">
						<img :src="item.iconUrl" alt="">
						<view>
							{{item.name}}
						</view>
					</view>
				</view>


			</scroll-view>
		</view>
	</view>
	<!-- 推荐歌单 -->
	<view class="copy">
		<view class="copy-top">
			<view>
				推荐歌单
			</view>
			<view>
				.......
			</view>
		</view>
		<scroll-view class="copy-content" scroll-x="true" scroll-left="120" :show-scrollbar="0">
			<view class="content-box" v-for="(item , index) in data.songSheet" :key="item.id">
				<view>
					<img :src="item.picUrl" alt="" @click=handlerSong(item)>
					<view>
						{{item.name}}
					</view>
				</view>
			</view>
		</scroll-view>
	</view>


	<view class="copySong">
		<view class="copy-top">
			<view>
				新歌速递
			</view>
			<view>
				......
			</view>
		</view>
		<view class="content-song" v-for="(item , index) in data.newSong" :key="item.id" @click=handlerSong(item)>
			<view class="text">
				<img :src="item.picUrl" :alt="item.name">
				<p>{{item.name}}</p>
			</view>
			<view class="iocn">
				播放按钮
			</view>
		</view>
	</view>

	<zero-loading v-if="loading" type="love"></zero-loading>




	<view class="play" @click="goPlay">
		<view class="content">
			<view class="img-song">
			<img src="https://p2.music.126.net/jS92kW49E37VDr8f6A1ZKg==/109951167550777780.jpg" alt="">
			<p>这世界这么大</p>
			</view>
			<view class="list">
				按钮+列表
			</view>
		</view>
	</view>
	
</template>



<script setup>
	import {
		reactive,
		ref
	} from 'vue'
	import {
		onLoad,
		onPullDownRefresh,
		onReachBottom,
		onShow,
	} from "@dcloudio/uni-app"
	import {
		Burl
	} from '../../utils/beasUrl.js'

	const data = reactive({
		banners: [],
		logoList: [],
		songSheet: [],
		newSong: []
	})
	const search = ref('')
	const loading = ref(false)
	const newNum = ref(null)
	
	const getBanner = () => {
		uni.request({
			url: `${Burl}/banner?type=2`,
			success: (res) => {
				data.banners = res.data.banners
			}
		})
	}
	const handlerBanner = (item) => {

		const go = () => window.location.href = item.url
		const song = () => {
			console.log(item)
			goPlay(item.targetId)
		}
		item.url ? go() : song()
	}
	const getsongSheet = () => {
		uni.request({
			url: Burl + "/personalized",
			data: {
				limit: 6
			},
			success: ((res) => {
				data.songSheet = res.data.result
			})
		})
	}
	const handlerSong = (item) => {
		// goPlay(item.id)
	}
	const getFindLogo = () => {

		uni.request({
			url: Burl + "/homepage/dragon/ball",
			success: (res) => {

				data.logoList = res.data.data
			}
		})
	}
	const getNewSong = () => {
		newNum.value += 9
		uni.request({
			url: Burl + `/personalized/newsong?limit=${newNum.value}`,
			success: (res) => {
				data.newSong = res.data.result
				// console.log(res.data.result)
			}
		})
	}
	const goPlay = (songId) => {
		uni.navigateTo({
			url: '../play/play?id='+songId,
			
		});
	}
	const goSearch = () => {
		uni.navigateTo({
			url: '../search/search'
		});
	}
	onPullDownRefresh(() => {
		console.log('下拉刷新');
		data.newSong = []
		newNum.value = null
		getNewSong()
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
	})
	onReachBottom(() => {
		console.log("滑下要数据");
		loading.value = true
		console.log(loading.value);
		setTimeout(() => {
			loading.value = false
			getNewSong()
		}, 2000)
	})
	onLoad(() => {
		getBanner()
		getsongSheet()
		getFindLogo()
		getNewSong()
	})
</script>

<style lang="scss" scoped>
	* {
		box-sizing: border-box;
	}

	.search {
		height: 70rpx;
		overflow: hidden;
		border-radius: 20rpx;

		input {
			margin-left: 15rpx;
			padding-left: 50rpx;
			border-radius: 20rpx;
			height: 100%;
			width: 100%;
			background-color: #ccc;
		}
	}

	.logoBox {
		::v-deep .item-box {
			white-space: nowrap;
			width: 100%;

			display: flex;
			justify-content: space-around;

			.uni-scroll-view-content {
				display: flex;
			}

			.item-box-content {
				background-color: gainsboro;
				display: flex;
				flex-direction: column;
				justify-content: space-around;
				width: 150rpx;
				margin: 0 30rpx;
				text-align: center;
				font-size: 5rpx;

				view {
					display: inline-block;
				}

				.box_log {
					display: flex;
					flex-direction: column;
					justify-content: space-around;

					img {
						width: 100rpx;
					}
				}
			}
		}
	}


	.uni-margin-wrap {
		width: 690rpx;
		width: 100%;

		.swiper {
			height: 300rpx;

			input {
				border-radius: 10rpx;
			}
		}

		.swiper-item {
			display: block;
			height: 300rpx;
			line-height: 300rpx;
			text-align: center;

			img {
				width: 100%;
				height: 100%;
			}

		}

	}

	.copy {
		::v-deep::-webkit-scrollbar {
			display: none;
			width: 0 !important;
			height: 0 !important;
			-webkit-appearance: none;
			background: transparent;
		}

		.copy-top {
			display: flex;
			justify-content: space-between;
		}

		.copy-content {

			white-space: nowrap;
			width: 100%;

			.scroll-view-item {
				height: 300rpx;
				line-height: 300rpx;
				text-align: center;
				font-size: 36rpx;
			}

			.content-box {
				display: inline-block;
				margin-right: 10rpx;
				width: 250rpx;
				// height: 250rpx;
				text-align: center;

				view {
					display: flex;
					flex-direction: column;
					justify-content: space-between;
					font-size: 20rpx;

					img {
						width: 100%;
						height: 100%;
						border-radius: 10rpx;
					}

					view {
						text-align: left;
						white-space: pre-wrap;
					}
				}
			}
		}

	}

	.copySong {
		margin-bottom: 110rpx;

		.copy-top {
			display: flex;
			justify-content: space-between;
		}

		.content-song {
			display: flex;
			width: 100%;
			height: 100rpx;
			justify-content: space-between;
			line-height: 100rpx;

			.text {
				display: flex;
				line-height: 100rpx;
				margin-left: 10rpx;
				margin-bottom: 10rpx;

				p {
					padding-left: 33rpx;
					white-space: nowrap;
					overflow: hidden;
					text-overflow: ellipsis;
					width: 480rpx;

				}

				img {

					height: 100rpx;
					border-radius: 20%;
				}

			}
		}
	}

	.play {
		background-color:#ccc;
		position: fixed;
		bottom: calc(var(--window-bottom));
		
		.content {
			display: flex;
			width: 750rpx;
			height: 100rpx;
			line-height: 100rpx;
			justify-content: space-between;
			.img-song{
				display: flex;
				margin-left: 10rpx;
				p{
					margin-left: 20rpx;
				}
				img{
				border-radius: 50%;
				height: 100rpx;
				width:100rpx
				}
			}
		}
	}
</style>
