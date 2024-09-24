<template>
    <li>
      <p @click="toggleComplete" :style="{color: task.completed ? 'green' : 'black' }">{{ task.text }}</p>
      <button @click="onEdit">edit</button>
      <button @click="onDelete">delete</button>
    </li>
  </template>
  
  <script>
  export default {
    props: {
      task: Object,
      index: Number,
    },
    emits: ['edit', 'delete', 'toggle-complete'],
    methods: {
      toggleComplete() {
        this.$emit('toggle-complete', this.task);
      },
      onEdit() {
        this.$emit('edit', this.index);
      },
      onDelete() {
        this.$emit('delete', this.index);
      },
    },
  };
  </script>
  