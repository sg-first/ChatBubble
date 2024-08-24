<template>
	<div v-if="message" class="message-box">
		{{ message }}
	</div>
</template>

<script>
	export default {
		props: {
			message: {
				type: String,
				default: ''
			}
		},
		methods: {
			showMessage(message, duration, callback) {
				this.$emit('update:message', message);
				setTimeout(() => {
					this.$emit('update:message', '');
					if (callback) {
						callback();
					}
				}, duration);
			}
		}
	};
</script>

<style scoped>
	.message-box {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background-color: rgba(0, 0, 0, 0.5);
		color: white;
		padding: 10px 20px;
		border-radius: 8px;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
		z-index: 1000;
	}
</style>