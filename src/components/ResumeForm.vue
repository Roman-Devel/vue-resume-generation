<template>
	<form class="card card-w30" @submit.prevent="submit">
		<div class="form-control">
			<label for="type">Тип блока</label>
			<select id="type" v-model="type">
				<option value="title">Заголовок</option>
				<option value="subtitle">Подзаголовок</option>
				<option value="avatar">Аватар</option>
				<option value="text">Текст</option>
			</select>
		</div>

		<div class="form-control">
			<label for="value">Значение</label>
			<textarea id="value" rows="3" v-model.trim="value"></textarea>
		</div>

		<button class="btn primary" :disabled="isDisabled">Добавить</button>
	</form>
</template>

<script>
export default {
	emits: ['addBlock'],
	data() {
		return {
			type: 'title',
			value: ''
		}
	},
	methods: {
		submit() {
			this.$emit('addBlock', {
				type: this.type,
				value: this.value
			})

			this.type = 'title'
			this.value = ''
		}
	},
	computed: {
		isDisabled() {
			return this.value.length < 4
		}
	}
}
</script>