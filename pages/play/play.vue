<template>
	<view class="img">
		<view class="needle">
			<img src="../../static/paly/needle-ab.png" alt="">
		</view>
		<view class="song">
			<img src="../../static/paly/yuan.png" alt="">
			<img src="https://p2.music.126.net/jS92kW49E37VDr8f6A1ZKg==/109951167550777780.jpg" id="song-img" alt="">
		</view>
	</view>
	<view class="play">
		<div class='iocn'
		@click = playWay
		><img src="../../static/paly/loop1-black.png" alt=""></div>
		<view class="button">
			<div
			@click=handlerLastsong
			><img src="../../static/paly/back.png" alt=""></div>

			<div v-show="playButton"
			@click = handlerplayButton
			><img src="../../static/paly/play-black.png" alt=""></div>
			<div v-show="!playButton"
			@click = handlerplayButton
			><img src="../../static/paly/pause.png" alt=""></div>

			
			
			<div
			@click=handlerNextsong
			><img id="rigth-button" src="../../static/paly/back.png" alt=""></div>
		</view>
		<div class='iocn'
		@click = handlerSongList
		><img src="../../static/paly/list-black.png" alt=""></div>
	</view>
</template>

<script setup>
	import {reactive , ref} from 'vue'
	import { onLoad } from "@dcloudio/uni-app"
	import { Burl } from "@/utils/beasUrl.js"
	const playButton = ref(false)
	const songData = reactive({
		songId : null,
		data : []
	})
	const handlerplayButton = () => {
		playButton.value = !playButton.value
	}
	const handlerLastsong = () =>{
		console.log("上一首");
	}
	const handlerNextsong = () =>{
		console.log("下一首");
	}
	const handlerSongList = () => {
		console.log("播放列表");
	}
	const playWay = ()=>{
		console.log("播放方式");
	}
	const getSong = () => {
		uni.request({
			url: `${Burl}/song/url?id=${songData.songId}`,
			success: (res) => {
			songData.data = res.data.data
			}
		})
	}
	const song = () =>{
		// const innerAudioContext = uni.createInnerAudioContext();
		// innerAudioContext.autoplay = true;
		// innerAudioContext.src = songData.data.url || ''
		// innerAudioContext.onPlay(() => {
		//   console.log('开始播放');
		// });
		// innerAudioContext.onError((res) => {
		//   console.log(res.errMsg);
		//   console.log(res.errCode);
		// });
	}
	onLoad((option) => {
		console.log(option);
		songData.songId = option.id
		getSong()
		song()
	})
</script>

<style lang="scss">
	.img {
		.song {
			width: 550rpx;
			height: 550rpx;
			position: absolute;
			top: 200rpx;
			margin-left: 100rpx;
			z-index: 666;

			img {
				width: 100%;
				height: 100%;
			}

			#song-img {
				position: absolute;
				top: 0;
				bottom: 0;
				left: 0;
				right: 0;
				margin: auto;
				width: 400rpx;
				height: 400rpx;
				border-radius: 50%;
				z-index: -1;

			}
		}

		.needle {
			position: absolute;
			z-index: 9999;
			top: 0;
			left: 285rpx;
			width: 180rpx;
			height: 300rpx;

			img {
				width: 100%;
				height: 100%;
				transform-origin: top left;
				// transform: rotateZ(-30deg);
			}
		}
	}

	.play {
		display:flex;
		position: relative;
		top: 950rpx;
		justify-content: space-between;
		.iocn {
			img {
				width: 80%;
				height: 80%;
			}
		}
		.button{
			display: flex;
			img {
				width: 80%;
				height: 80%;
				margin: 0 15rpx;
			}
			#rigth-button{
				transform: rotateY(180deg);
			}
		}
	}
</style>
