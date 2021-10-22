<template>
	<div class="container column">
		<ResumeForm @addBlock="addBlock" />
		<AppLoader v-if="!isActive" />
		<ResumeView
			:blocks="componentsList"
			:isActive="isActive"
		/>
	</div>
	<div class="container">
		<ResumeComments
			@load="loadComments"
			:comments="comments"
		/>
		<AppLoader v-if="isLoad" />
	</div>
</template>

<script>
import ResumeForm from './components/ResumeForm.vue'
import ResumeComments from './components/ResumeComments.vue'
import ResumeView from './components/ResumeView.vue'
import AppLoader from './components/AppLoader.vue'
import axios from 'axios'

export default {
	data: () => ({
		componentsList: [],
		comments: [],
		isLoad: false,
		isActive: false
	}),
	async mounted() {
		try {
			const {data} = await axios.get(process.env.VUE_APP_DB_URL)

			this.componentsList = Object.keys(data).map(key => {
				return {
					id: key,
					...data[key]
				}
			})
		} catch (e) { console.log('Список блоков пуст!', e.message) }

		setTimeout(() => { this.isActive = true }, 300)
	},
	methods: {
		async addBlock({type, value}) {
			const {data} = await axios.post(process.env.VUE_APP_DB_URL, {
				currentComponent: `resume-${type}`,
				[type]: value
			})

			this.componentsList.push({
				id: data.name,
				currentComponent: `resume-${type}`,
				[type]: value
			})
		},
		loadComments() {
			this.isLoad = true
			setTimeout(async () => {
				try {
					const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
					this.comments = data
					this.isLoad = false
				} catch (e) { console.log(e.message) }
			}, 800)
		}
	},
	components: {
		ResumeForm,
		ResumeComments,
		ResumeView,
		AppLoader
	}
}
</script>