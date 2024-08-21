<template>
	<div class="centered-container">
		<div class="main-content">
			<!-- 聊天记录部分 -->
			<div class="chat-history">
				<div v-if="chats && chats.length > 0">
					<div v-for="record in chats" :key="record.id">
						<ChatBubble :record="record" />
					</div>
				</div>
				<div v-show="chats && chats.length > 0" class="footer">
					<button @click="clearChatHistory" class="clear-button">清空聊天记录</button>
				</div>
			</div>
			
			<!-- 对话文本框输入部分 -->
			<div class="chat-input-container">
				<textarea v-model="newMessage" placeholder="请输入消息..." class="chat-input-textarea"></textarea>
				<button @click="sendMessage" class="send-button">发送</button>
			</div>
			
		</div>
	</div>
</template>

<script>
	import axios from 'axios';
	import ChatBubble from "@/components/ChatBubble.vue";
	
	export default {
		components: {
			ChatBubble
		},
		
		data() {
			return {
				uid: '',
				cid: '',
				chats: [],
				newMessage: '',
			};
		},
		
		mounted() 
		{
			this.uid = localStorage.getItem('uid');
			this.cid = this.$route.query.cid;
			
			const storedChats = localStorage.getItem(`${this.uid}-${this.cid}-chats`);
			if (storedChats)
			{
				this.chats = JSON.parse(storedChats);
			}
			
		},
		
		sendMessage()
		{
			if (this.newMessage !== '') 
			{
				const record = {
					speaker: 'user',
					msg: this.newMessage.trim()
				};
		
				this.chats.push(record);
		
				// TODO: 发送消息到WebSocket服务器
		
				this.saveChatToLocalStorage();
				this.newMessage = ''; // 清空输入框
			}
		},
		
		saveChatToLocalStorage() 
		{
			localStorage.setItem(`${this.uid}-${this.cid}-chats`, JSON.stringify(this.chats));
		},
		
		clearChatHistory() 
		{
			// 清空聊天记录
			if (this.chats) 
			{
				this.chats = [];
				this.saveChatToLocalStorage();
			}
		},
	}	
</script>

<style>
</style>
