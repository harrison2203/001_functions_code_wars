<script setup>
import { ref, defineProps, defineEmits } from 'vue';

const inputName = ref('');
const { message } = defineProps(['message']);
const emits = defineEmits(['inputChanged']);

const inputNameInfo = ref({
  value: inputName,
  errorMessage: '',
	invalid: null
});

function onInputChange(){

	if(inputName.value !== ""){
		emits('inputChanged', inputName.value);

		if(inputName.value.match(/[-_;,:!]/gi)){
			inputNameInfo.value.errorMessage = "those values are forbidden";
			inputNameInfo.value.invalid = true;

		}else if(inputName.value.match(/[0-9]/gi)){
			inputNameInfo.value.errorMessage = "Numbers are not allowed";
			inputNameInfo.value.invalid = true;
		}
	}
}
</script>

<template>
	<div>
		<div>
			<slot>
					<label for="name-input" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">{{ message }}</label>
			</slot>
			<input
					id="name-input"
					class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
					:class="{'border border-red-500 border-2': inputNameInfo.invalid}" 
					v-model="inputName"
					@input="onInputChange"
			/>
		</div>
				<p v-if="inputNameInfo.errorMessage" class="text-base text-red-500">
					{{ inputNameInfo.errorMessage }}
				</p>
	</div>
</template>

