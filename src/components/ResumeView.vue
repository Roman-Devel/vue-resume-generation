<template>
	<div class="card card-w70" v-show="isActive">
		<template v-for="block in blocks" :key="block.id">
			<component :is="block.currentComponent" v-bind="block"></component>
		</template>

		<h3 v-if="!blocks.length">Добавьте первый блок, чтобы увидеть результат</h3>
	</div>
</template>

<script>
import ResumeTitle from './parts/ResumeTitle.vue'
import ResumeAvatar from './parts/ResumeAvatar.vue'
import ResumeSubtitle from './parts/ResumeSubtitle.vue'
import ResumeText from './parts/ResumeText.vue'

export default {
	props: {
		'blocks': {
			type: Array,
			required: true
		},
		isActive: {
			type: Boolean,
			required: true
		}
	},
	components: {
		ResumeTitle,
		ResumeAvatar,
		ResumeSubtitle,
		ResumeText
	}
}
</script>