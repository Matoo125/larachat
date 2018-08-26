<template>
	<div class="chat card">
		<div class="card-body">
			
			<chat-messages></chat-messages>
			<div class="chat__message_input">
				<form action="#">
				<div class="form-group">
					<textarea
						id="body"
						cols="30"
						rows="4"
						class="chat__form-input form-control"
						v-model="body"
						@keydown="handleMessageInput"
					></textarea>
				</div>
				<span class="chat__form-helptext">
					Hit return to send or Shift + return for a new line
				</span>
			</form>
			</div>
		</div>
	</div>
</template>

<script>
	import Bus from '../../bus'
	import moment from 'moment'

	export default {
		data () {
			return {
				body: null,
				bodyBackedUp: null
			}
		},
		methods: {
			handleMessageInput (e) {
				this.bodyBackedUp = this.body

				if (e.keyCode === 13 && !e.shiftKey) {
					e.preventDefault();
					this.send();
				}
			},
			buildTempMessage () {
				let tempId = Date.now();

				return {
					id: tempId,
					body: this.body,
					created_at: moment().utc(0).format('YYY-MM-DD HH:mm:ss'),
					selfOwned: true,
					user: {
						name: Laravel.user.name
					}
				}
			},
			send () {
				if (!this.body || this.body.trim() === '') {
					return
				}

				let tempMessage = this.buildTempMessage();
				

				Bus.$emit('message.added', tempMessage)

				axios.post('/chat/messages', {
					body: this.body.trim()
				}).catch( () => {
					this.body = this.bodyBackedUp
					Bus.$emit('message.removed', tempMessage)
				})

				this.body = null
			}
		}
	}
</script>

<style type="scss">

</style>