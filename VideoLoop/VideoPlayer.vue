<template>
	<div class="video-player-card" v-if="videoUrl">
		<!-- 第一个视频播放 -->
		<video id="videoPlayerStart" class="video" preload="auto" ref="videoPlayerStart" @canplay="checkBufferAndPlay"
			@ended="videoLoop" :class="{ hidden: currentVideo !== 1 }"><!-- 当视频不为1时隐藏 -->
			<source :src="videoUrl" type="video/mp4">
		</video>

		<!-- 第二个视频播放 -->
		<video id="videoPlayerBack" class="video" preload="auto" ref="videoPlayerBack" @ended="videoLoop"
			:class="{ hidden: currentVideo !== 2 }">
			<source :src="backUrl" type="video/mp4">
		</video>

	</div>
	<!-- 图片显示 -->
	<img v-else :src="pictureSrc" alt="Character Picture" class="character-picture">
</template>

<script>
	export default {
		props: {
			videoUrl: String,
			backUrl: String,
			pictureSrc: String,
		},
		data() {
			return {
				currentVideo: 1, // 当前播放的视频
			};
		},
		methods: {
			checkBufferAndPlay() {
				const video = this.$refs.videoPlayerStart;
				//防止buffered的报错
				if (!video || !video.buffered || video.buffered.length === 0) {
					// 如果 video 还没有完全准备好，延迟重试
					setTimeout(this.checkBufferAndPlay, 500);
					return;
				}
				const buffered = video.buffered;
				const duration = video.duration;

				// 确保已经缓冲了足够的内容（至少 10 秒或视频总时长的 50%）
				if (buffered.length > 0 && (buffered.end(0) >= Math.min(10, duration * 0.5))) {
					video.play();
				} else {
					// 如果未完全缓冲，延迟播放并继续检查缓冲进度
					setTimeout(this.checkBufferAndPlay, 500);
				}
			},
			videoLoop() {
				// 切换到下一个视频或重新开始播放
				if (this.currentVideo === 1) {
					this.currentVideo = 2; // 切换到第二个视频
				    this.$refs.videoPlayerBack.play();
				} else {
					this.currentVideo = 1; // 重置为第一个视频
					this.$refs.videoPlayerStart.currentTime = 0;
					this.$refs.videoPlayerStart.play(); // 重新开始播放第一个视频
				}
			}
		},
	};
</script>

<style>
	.video-player-card {
		position: relative;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.video {
		max-height: 300px;
		max-width: 400px;
		width: auto;
		height: auto;
	}

	.hidden {
		display: none;
	}

	.character-picture {
		max-height: 300px;
		max-width: 350px;
		width: auto;
		height: auto;
		display: block;
		margin: auto;
	}
</style>