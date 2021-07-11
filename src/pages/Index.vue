<template>
	<div class="q-pa-md q-gutter-sm">
		<q-editor
			v-model="editor"
			min-height="5rem"
			:definitions="{
				save: {
					tip: 'Save your task',
					icon: 'save',
					label: 'Save',
					handler: saveTask,
				},
			}"
			:toolbar="[['bold', 'italic', 'strike', 'underline'], ['save']]"
		/>

		<q-card
			flat
			bordered
			v-for="(item, index) in tasks"
			:key="'task_' + index"
			class="row"
		>
			<q-card-section
				v-html="item.texto"
				class="col"
				:class="item.state ? 'tachar' : ''"
			/>
			<q-btn flat color="blue" @click="item.state = !item.state">
				<span v-if="!item.state">Completed</span>
				<span v-else>Pending</span>
			</q-btn>
			<q-btn flat color="red" @click="removeTask(index)"> Remove </q-btn>
		</q-card>
		<div v-if="!tasks.length">
			<q-banner rounded class="bblue text-white"> No tasks. </q-banner>
		</div>
	</div>
</template>

<script>
import { ref } from "vue";
import { useQuasar } from "quasar";
import App from "src/App.vue";

export default {
	/* 	components: { App }, */
	setup() {
		const $q = useQuasar();
		const editor = ref("Next <b>task</b>");
		let tasks = ref([
			{
				texto: "Task #1",
				state: false,
			},
			{
				texto: "Task #2",
				state: true,
			},
			{
				texto: "Task #3",
				state: true,
			},
		]);

		return {
			editor,
			tasks,
			saveTask() {
				tasks.value.push({
					texto: editor.value,
					state: false,
				});
				$q.notify({
					type: "positive",
					message: "Task saved !!",
				});
				editor.value = "";
			},
			removeTask(index) {
				$q.dialog({
					title: "Danger !!",
					message: "Do you really want to remove the task?",
					cancel: true,
					persistent: true,
				}).onOk(() => {
					tasks.value.splice(index, 1);
					$q.notify({
						type: "negative",
						message: "Task removed !!",
					});
				});
			},
		};
	},
};
</script>

<style scoped>
.tachar {
	text-decoration: line-through;
}
</style>
