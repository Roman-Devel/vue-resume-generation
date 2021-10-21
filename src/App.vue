<template>
	<div class="container column">
		<app-form
			v-model="type"
			v-model:form-value="formValue"
			@addBlock="addBlock()"
		></app-form>

		<app-loader v-if="!isActive"></app-loader>
		<div class="card card-w70" v-show="isActive">
				<div v-for="item in componentsList" :key="item.id">
					<component :is="item.currentComponent" v-bind="item"></component>
				</div>

				<h3 v-if="!componentsList.length">Добавьте первый блок, чтобы увидеть результат</h3>
		</div>
	</div>
	<div class="container">
		<p>
			<button class="btn primary" @click="load">Загрузить комментарии</button>
		</p>
		<app-comments :comments="comments"></app-comments>
		<app-loader v-if="isLoad"></app-loader>
	</div>
</template>

<script>
import AppForm from './components/AppForm.vue'
import AppTitle from './components/AppTitle.vue'
import AppAvatar from './components/AppAvatar.vue'
import AppSubtitle from './components/AppSubtitle.vue'
import AppText from './components/AppText.vue'
import AppComments from './components/AppComments.vue'
import AppLoader from './components/AppLoader.vue'
import axios from 'axios'

export default {
	data: () => ({
		type: 'title',
		formValue: '',
		title: '',
		avatar: '',
		subtitle: '',
		text: '',
		componentsList: [],
		comments: [],
		isLoad: false,
		isActive: false
	}),
	async mounted() {
		try {
			const {data} = await axios.get('https://vue-coursework-2-default-rtdb.firebaseio.com/blocks.json')
		
			this.componentsList = Object.keys(data).map(key => {
				return {
					id: key,
					...data[key]
				}
			})
		} catch (error) {
			console.log('Список блоков пуст!', error.message)
		}

		setTimeout(() => {
			this.isActive = true
		}, 300)
	},
	methods: {
		async addBlock() {
			const {data} = await axios.post('https://vue-coursework-2-default-rtdb.firebaseio.com/blocks.json', {
				currentComponent: this.currentComponent,
				[this.type]: this.formValue
			})

			this.componentsList.push({
				id: data.name,
				currentComponent: this.currentComponent,
				[this.type]: this.formValue
			})
			this.formValue = ''
		},
		load() {
			this.isLoad = true
			setTimeout(async () => {
				try {
					const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
					this.comments = data
					this.isLoad = false
				} catch (error) {
					console.log(error)
				}
			}, 800)
		}
	},
	computed: {
		currentComponent() {
			return `app-${this.type}`
		}
	},
	components: {
		AppForm,
		AppTitle,
		AppAvatar,
		AppSubtitle,
		AppText,
		AppComments,
		AppLoader
	}
}
</script>

<style>
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