<template>
	<view class="footer">
		<view class="footer-left">
			<view @tap="startRecognize">
				<fa-FontAwesome type="fas fa-microphone"></fa-FontAwesome>
			</view>
		</view>
		<view class="footer-center">
			<input class="input-text" 
				@confirm="sendMessge"
				type="text" 
				v-model="inputValue"
				placeholder="write a message..."
			></input>
		</view>
		<view class="footer-right" @tap="sendMessge">
			<view id='msg-type'>发送</view>
		</view>
	</view>
</template>

<script>
export default {
	name: "chat-input",
	data() {
		return {
			inputValue: ''
		}
	},
	computed: {
		i18n: {
			get() { return this.$_i18n.messages[this.$_i18n.locale];},
		},
	},
	methods: {
		startRecognize: function () {
			var options = {};
			var that = this;
			options.engine = 'iFly';
			that.inputValue = "";
			plus.speech.startRecognize(options, function (s) {
				console.log(s);
				that.inputValue += s;
			}, function (e) {
				console.log("语音识别失败：" + e.message);
			});
		},
		sendMessge: function () {
			var that = this;
			if (that.inputValue.trim() == '') {
				this.uShowToast(this.i18n.components.enterContent);
				that.inputValue = '';
			} else {
				//点击发送按钮时，通知父组件用户输入的内容
				this.$emit('send-message', {
					type: 'text',
					msgContent: that.inputValue
				});
				that.inputValue = '';
			}
		},
	}
}
</script>

<style>
	.footer {
		display: flex;
		flex-direction: row;
		width: 100%;
		height: 80rpx;
		min-height: 80rpx;
		border-top: solid 1rpx #bbb;
		overflow: hidden;
		padding: 5rpx;
		background-color: #fafafa;
	}
	.footer-left {

		width: 80rpx;
		height: 80rpx;

		display: flex;
		justify-content: center;
		align-items: center;
	}
	.footer-right {
		width: 120rpx;
		height: 80rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #1482D1;
	}
	.footer-center {
		flex: 1;
		height: 80rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.footer-center .input-text {
		flex: 1;
		background: #fff;
		border: solid 1rpx #ddd;
		padding: 10rpx !important;
		font-family: verdana !important;
		overflow: hidden;
		border-radius: 15rpx;
	}
</style>
