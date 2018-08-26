<template>
	<div class="chat__message list-group-item" :class="{ 'active': !message.selfOwned}">
		<strong class="chat__message-user">{{ message.user.name }}</strong>
		<span class="chat__message-timestamp">{{ message.created_at }}</span>
		<p class="chat__message-body">{{ message.body }}</p>
	</div>
</template>

<script>
	export default {
		props: ['message']
	}
</script>

<style type="scss">


</style>