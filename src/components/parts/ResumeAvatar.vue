<template>
	<div class="avatar">
		<img :src="avatar" v-if="isValid">
	</div>
</template>

<script>
export default {
	data() {
		return {
			avatar: this.$attrs.avatar
		}
	},
	computed: {
		isValid() {
			return typeof this.avatar === 'string' && this.avatar.length
		}
	}
}
</script>

<style scoped>
	.avatar {
		display: flex;
		justify-content: center;
	}

	.avatar img {
		width: 150px;
		height: auto;
		border-radius: 50%;
	}
</style>