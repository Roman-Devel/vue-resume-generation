<template>
	<form class="card card-w30" @submit.prevent="$emit('addBlock')">
		<div class="form-control">
			<label for="type">Тип блока</label>
			<select id="type" :value="modelValue" @input="change">
				<option value="title">Заголовок</option>
				<option value="subtitle">Подзаголовок</option>
				<option value="avatar">Аватар</option>
				<option value="text">Текст</option>
			</select>
		</div>

		<div class="form-control">
			<label for="value">Значение</label>
			<textarea id="value" rows="3" :value="formValue" @input="changeValue"></textarea>
		</div>

		<button class="btn primary" :disabled="isDisabled">Добавить</button>
	</form>
</template>

<script>
export default {
	emits: {
		'update:modelValue': null,
		'update:formValue': null,
		'addBlock': null
	},
	props: {
		modelValue: {
			type: String,
			required: true
		},
		formValue: {
			type: String,
			required: true
		}
	},
	methods: {
		change(event) {
			this.$emit('update:modelValue', event.target.value)
		},
		changeValue(event) {
			this.$emit('update:formValue', event.target.value.trim())
		}
	},
	computed: {
		isDisabled() {
			return this.formValue.length < 4
		}
	}
}
</script>