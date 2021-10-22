<template>
	<h1 v-if="isValid">{{ title }}</h1>
</template>

<script>
export default {
	data() {
		return {
			title: this.$attrs.title
		}
	},
	computed: {
		isValid() {
			return typeof this.title === 'string' && this.title.length
		}
	}
}
</script>