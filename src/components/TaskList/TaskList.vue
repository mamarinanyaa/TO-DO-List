<template>
    <ul>
        <TaskItem
      v-for="(item, index) in tasks"
      :key="index"
      :task="item"
      :index="index"
      @edit="editTask"
      @delete="deleteTask"
      @toggle-complete="toggleComplete"
    />
    </ul>
</template>

<script>
    import TaskItem from './TaskItem/TaskItem.vue';

    export default {
    components: {
        TaskItem,
    },
    props: {
        tasks: Array,
    },
    emits: ['edit', 'delete', 'toggle-complete'],
    methods: {
        editTask(index) {
        this.$emit('edit', index);
        },
        deleteTask(index) {
        this.$emit('delete', index);
        },
        toggleComplete(task) {
        this.$emit('toggle-complete', task);
        },
    },
    };
</script>