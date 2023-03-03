<template>
<div>
	<form class="definition-input">
			<label class="input-title" for="textbox">{{ title }}</label>

			<input type="textbox" class="modern-textbox"
			v-model="name" 
      v-on:mouseup="sendValueUpdate"
			/>
	</form>

</div>

</template>

<script setup>
import { ref } from "vue";

const props = defineProps(["title"]);
const emits = defineEmits(['updateText']);

let name = ref('type text here');

function sendValueUpdate(){
  emits('updateText', name.value);
}

</script>

<style scoped>


</style>