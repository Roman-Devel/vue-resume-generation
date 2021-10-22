<template>
	<h2 v-if="isValid">{{ subtitle }}</h2>
</template>

<script>
export default {
	data() {
		return {
			subtitle: this.$attrs.subtitle
		}
	},
	computed: {
		isValid() {
			return typeof this.subtitle === 'string' && this.subtitle.length
		}
	}
}
</script>