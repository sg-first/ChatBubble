<template>
	<div class="message-row">
		<div :class="bubbleClass">
			<p>{{ record.msg }}</p>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			record: {
				type: Object,
				required: true
			}
		},
		computed: {
			bubbleClass() {
				return {
					'chat-bubble': true,
					'mine': this.record.speaker === 'user',
					'others': this.record.speaker === 'server'
				};
			}
		}
	};
</script>

<style>
	.message-row {
		clear: both;
		width: 100%;
		margin-bottom: 1px;
	}

	.chat-bubble {
		display: inline-block;
		position: relative;
		margin: 15px auto 0;
		border-radius: 10px;
		width: auto;
		max-width: 240px;
		height: auto;
		word-wrap: break-word;
	}

	.chat-bubble p {
		text-align: left;
		padding: 7px;
	}

	.chat-bubble.mine {
		background-color: rgb(64, 127, 252);
		float: right;
		color: white;
		/* 让用户发送的气泡靠右 */
	}

	.chat-bubble.others {
		background-color: rgb(243, 243, 247);
		float: left;
		/* 让服务器发送的气泡靠左 */
	}
</style>