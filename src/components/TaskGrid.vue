<template>
    <div class="task-grid">
        <template v-if="tasks.length">
            <Task v-for="(task, index) in tasks" :key="task.name" @taskDeleted="$emit('taskDeleted', index)" @taskStateChanged="$emit('taskStateChanged', index)" :task="task" />
        </template>
        <p v-else class="no-task">Sem atividades para fazer :)</p>
    </div>
</template>

<script>

    import Task from './Task.vue'

    export default {
        components: {
            Task
        },
        props: {
            tasks: {
                type: Array,
                required: true
            }
        }
    }
</script>

<style>
    .task-grid{
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }
    .task-grid .task {
        margin: 10px;
    }
    .no-task {
        color: #AAA;
        font-size: 1.7rem;
    }
</style>