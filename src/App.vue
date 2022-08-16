<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TasksProgress :progress="progress" />
		<NewTask @taskAdded="addTask" />
		<TaskGrid :tasks="tasks" @taskDeleted="deleteTask" @taskStateChanged="toggleTaskState" />
	</div>
</template>

<script>

	import TasksProgress from './components/TasksProgress.vue'
	import NewTask from './components/NewTask.vue'
	import TaskGrid from './components/TaskGrid.vue'

	export default {
		components: {
			TasksProgress,
			NewTask,
			TaskGrid
		},
		data() {
			return {
				tasks: []
			}
		},
		computed: {
			progress() {
				const tasksLength = this.tasks.length
				const tasksDone = this.tasks.filter(task => !task.pending).length
				return Math.round(tasksDone / tasksLength * 100) || 0
			}
		},
		watch: {
			tasks: {
				deep: true,
				handler() {
					localStorage.setItem('tasks', JSON.stringify(this.tasks))
				}
			}
		},
		methods: {
			addTask(item) {
				const sameTaskName = task => task.name === item.name
				const reallyNew = this.tasks.filter(sameTaskName).length == 0
				if(reallyNew) {
					this.tasks.push({
						name: item.name,
						pending: item.pending || true
					})
				}
			},
			deleteTask(index) {
				this.tasks.splice(index, 1)
			},
			toggleTaskState(index) {
				this.tasks[index].pending = !this.tasks[index].pending
			}
		},
		created() {
			const tasksFromLocalStorage = localStorage.getItem('tasks')
			const tasksParsed = JSON.parse(tasksFromLocalStorage)
			this.tasks = Array.isArray(tasksParsed) ? tasksParsed : []
		}
	}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}
	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}
	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>