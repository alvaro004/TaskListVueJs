<template>
	<div id="app">
		<section class="form">
			<form @submit.prevent="">
				<h1>TASKS APP</h1>
				<input
					@keypress.enter="indexBeingEdit === null ? createNewTask() : saveChanges()"
					v-model="newTask"
					type="text"
					placeholder="Write your task"
				/>
				<button type="button" @click="indexBeingEdit === null ? createNewTask() : saveChanges()">
					{{ indexBeingEdit === null ? `Create New Task` : `Save Changes` }}
				</button>
			</form>
		</section>
		<ul>
			<li v-for="(task, index) in tasks" :key="index">
				<span :class="{ 'task-selected': index === indexBeingEdit }"> {{ task }} </span>
				<div class="buttons">
					<button
						class="delete"
						type="button"
						:style="{ display: display }"
						:disabled="isActive"
						@click="deleteTask(index)"
					>
						Delete Task
					</button>
					<button class="edit" type="button" @click="editTask(index)">Edit Task</button>
				</div>
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	name: 'App',
	data() {
		return {
			indexBeingEdit: null,
			newTask: '',
			tasks: [],
			isActive: false,
			display: 'block',
		};
	},
	methods: {
		createNewTask() {
			if (this.newTask !== '') {
				this.tasks.push(this.newTask);
				this.newTask = '';
			} else {
				alert('You need to write a Task!');
			}
		},
		deleteTask(index) {
			this.tasks = this.tasks.filter((_, i) => index !== i);
		},
		editTask(index) {
			this.newTask = this.tasks[index];
			this.indexBeingEdit = index;
			this.isActive = true;
			this.display = 'none';
		},
		saveChanges() {
			this.tasks[this.indexBeingEdit] = this.newTask;
			this.newTask = '';
			this.indexBeingEdit = null;
			this.isActive = false;
			this.display = 'block';
		},
	},
};
</script>
