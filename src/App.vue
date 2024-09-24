<template>
  <div id="app">
    <h1>To-Do List</h1>

    <div class="input-container">
      <input v-model="newTask" placeholder="Add a new task" />
      <button @click="addTask">Add Task</button>

      <TaskList
        :tasks="tasks"
        @edit="editTask"
        @delete="deleteTask"
        @toggle-complete="toggleComplete"
      />

      <div v-if="editingIndex != null">
        <input v-model="editingTask" placeholder="Edit a task" />
        <button @click="saveChanges">Save changes</button>
      </div>
    </div>
  </div>
</template>

<script>
  import { ref, onMounted, watch } from "vue";
  import TaskList from './components/TaskList/TaskList.vue';

  export default {
    components: {
      TaskList,
    },
    setup() {
      const tasks = ref([]);
      const newTask = ref("");
      const editingIndex = ref(null);
      const editingTask = ref("");

      onMounted(() => {
        const savedTasks = localStorage.getItem("tasks");
        if (savedTasks) {
          tasks.value = JSON.parse(savedTasks);
        }
      });

      const addTask = () => {
        if (newTask.value.trim() === "") return;
        tasks.value.push({ text: newTask.value, completed: false });
        newTask.value = "";
      };

      const editTask = (index) => {
        editingIndex.value = index;
        editingTask.value = tasks.value[index].text;
      };

      const saveChanges = () => {
        if (editingTask.value.trim() === "") return;
        tasks.value[editingIndex.value].text = editingTask.value;
        editingIndex.value = null;
      };

      const deleteTask = (index) => {
        tasks.value.splice(index, 1);
      };

      const toggleComplete = (task) => {
        task.completed = !task.completed;
      };

      watch(tasks, (newTasks) => {
        localStorage.setItem("tasks", JSON.stringify(newTasks));
      }, { deep: true });

      return {
        newTask,
        tasks,
        editingIndex,
        editingTask,
        addTask,
        editTask,
        deleteTask,
        saveChanges,
        toggleComplete,
      };
    },
  };
</script>

