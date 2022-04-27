<template>
    <div :key="task.id" v-for="task in tasks">
        <Task @delete-task="$emit('delete-task',task.id)" :task="task" @toggle-reminder="$emit('toggle-reminder',task.id)"/>
    </div>
</template>

<script>
import Task from './Task';

export default {
    name:'Tasks',
    props: {
        tasks:Array,
    },
    components:{
        Task,
    },
    emits:['delete-task','toggle-reminder']
}
</script>
