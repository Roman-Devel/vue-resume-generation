<template>
	<p v-if="isValid">{{ text }}</p>
</template>

<script>
export default {
	data() {
		return {
			text: this.$attrs.text
		}
	},
	computed: {
		isValid() {
			return typeof this.text === 'string' && this.text.length
		}
	}
}
</script>