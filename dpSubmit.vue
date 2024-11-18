<script setup lang="ts">
import { notAfterDate, notBeforeDate, required, notAfterToday } from '@/main';
import DatePicker from '@/patterns/DatePicker/DatePicker.vue';
import { ref, reactive, provide } from 'vue';

const piece = reactive({
	dateRedaction: '',
	dateReception: '',
});
const date = ref('');

const isValid = ref(false);
const formSubmitted = ref(false);
provide('formSubmitted', formSubmitted);

const send = () => {
	formSubmitted.value = true;
	if (isValid.value) {
		console.log('send');
	}
};

const notAfterTodayRules = [notAfterToday];
</script>

<template>
	<v-form ref="form" v-model="isValid" @submit.prevent="send">
		<DatePicker
			v-model="piece.dateRedaction"
			label="Date de rédaction (optionnel)"
			bg-color="white"
			outlined
			:warning-rules="notAfterTodayRules"
			:rules="[required, notAfterDate(piece.dateReception || '')]"
		/>
		<br>
		<DatePicker
			v-model="piece.dateReception"
			label="Date de réception"
			bg-color="white"
			outlined
			:rules="[required, notBeforeDate(piece.dateRedaction || '')]"
		/>
		<br>
		<DatePicker
			v-model="date"
			label="Date"
			outlined
			:rules="[required, notBeforeDate(piece.dateRedaction || '')]"
		/>
		<v-btn type="submit">Submit</v-btn>
	</v-form>
</template>
