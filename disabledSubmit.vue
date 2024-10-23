<script lang="ts">
import NirField from '../src/patterns/NirField';
import { defineComponent } from 'vue';

export default defineComponent({
	components: {
		NirField,
	},
	data: () => ({
		isFormValid: false,
		value: null,
	}),
	methods: {
		submitForm() {
			if (this.$refs.form.validate()) {
				console.log('Form sent');
			}
		},
	},
});
</script>

<template>
	<VForm ref="form" v-model="isFormValid" @submit.prevent="submitForm">
		<NirField
			v-model="value"
			:nir-length="13"
			required
			@input="this.$refs.form.validate()"
		/>
		<VBtn color="primary" :disabled="!isFormValid" @click="submitForm">Submit</VBtn>
	</VForm>
</template>
