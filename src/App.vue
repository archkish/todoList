<template>
  <h2>ToDo List</h2>
  <form @submit.prevent="addTask">
    <input type="text" v-model="newTask" placeholder="Add a new task...">
    <button>Add Task</button>
  </form>
  <ul>
    <li v-for="(task, index) in tasks" :key="index">
      <input type="checkbox" v-model="task.completed">
      <span :class="{ completed: task.completed }">{{ task.title }}</span>
      <button @click="deleteTask(index)">Delete</button>
    </li>
  </ul>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      // let newTask = "";
      newTask: '',
      // let tasks = [{...}]
      tasks: [
        {
          title: 'add styles by design',
          completed: false
        },
        {
          title: 'add sort by complete',
          completed: false
        },
        {
          title: 'watch video about start vue3 install',
          completed: false
        },
        {
          title: 'add tasks in localStorage',
          completed: false
        },
      ]
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({title: this.newTask, completed: false});
        this.newTask = '';
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
  },
}
</script>

<style>

.completed {
  text-decoration: line-through;
}
</style>
