<template>
  <div class="tasks">
    <h2 class="tasks__title">ToDo List</h2>
    <form class="form" @submit.prevent="addTask">
      <input
        class="form__input"
        type="text"
        v-model="newTask"
        placeholder="Add a new task..."
      />
      <button class="form__btn">Add Task</button>
    </form>
    <ul class="list">
      <li class="list__item" v-for="(task, index) in tasks" :key="index">
        <input
          class="list__checkbox"
          type="checkbox"
          v-model="task.completed"
          v-on:change="checkTaskComplete(index, 'noCompleted')"
        />
        <input
          class="list__editing"
          type="text"
          @keyup.enter="editTask(task.title), doCheck(task)"
          v-if="task.isEditing"
          v-model="editValue"
        />
        <span
          else
          class="list__task"
          :class="{ completed: task.completed, isEditing: task.isEditing }"
        >
          {{ task.title }}
        </span>
        <div class="wrapper__btn">
          <div class="wrapper">
            <button
              class="list__btn"
              v-if="check"
              :class="{ check: check }"
              @click="editTask(task.title), doCheck(task)"
            ></button>
            <button
              else
              class="list__btn list__btn_correction"
              :class="{ check: check, hide: check }"
              @click="changeEditing(task.title), doCheck(task)"
            ></button>
          </div>
          <div>
            <button
              class="list__btn list__btn_close"
              v-if="this.check"
              @click="changeEditing(task.title), doCheck(task)"
            ></button>
            <button
              else
              :class="{ hide: check }"
              class="list__btn"
              @click="deleteTask(index)"
            ></button>
          </div>
        </div>
      </li>
    </ul>

    <hr class="devider" />
    <h2 class="tasks__title tasks__title_left">Done</h2>

    <ul class="list" id="done">
      <li class="list__item" v-for="(task, index) in doneTasks" :key="index">
        <input
          class="list__checkbox"
          type="checkbox"
          checked
          v-on:change="checkTaskComplete(index, 'complete')"
          v-model="task.completed"
        />
        <span class="list__show" :class="{ completed: task.completed }">{{
          task.title
        }}</span>
        <div class="wrapper__btn">
          <button class="list__btn" @click="deleteDoneTask(index)"></button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
// import { thisTypeAnnotation } from '@babel/types';

export default {
  name: "App",
  data() {
    return {
      newTask: "",
      editValue: "",
      tasks: [
        {
          title: "add styles by design",
          completed: false,
          isEditing: false,
          check: false,
        },
        {
          title: "add sort by complete",
          completed: false,
          isEditing: false,
          check: false,
        },
        {
          title: "watch video about start vue3 install",
          completed: false,
          isEditing: false,
          check: false,
        },
        {
          title: "add tasks in localStorage",
          completed: false,
          isEditing: false,
          check: false,
        },
      ],
      doneTasks: [],
    };
  },

  mounted() {
    if (localStorage.doneTasks) {
      this.doneTasks = JSON.parse(localStorage.doneTasks);
    }

    if (localStorage.tasks) {
      this.tasks = JSON.parse(localStorage.tasks);
    }
  },

  watch: {
    tasks: {
      handler(newList) {
        localStorage.tasks = JSON.stringify(newList);
      },
      deep: true,
    },
    doneTasks: {
      handler(newDoneList) {
        localStorage.doneTasks = JSON.stringify(newDoneList);
      },
      deep: true,
    },
  },

  methods: {
    doCheck() {
      this.check = !this.check;
    },

    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.unshift({
          title: this.newTask,
          completed: false,
          editing: false,
        });

        this.newTask = "";
      }
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    deleteDoneTask(index) {
      this.doneTasks.splice(index, 1);
    },

    changeEditing(taskText) {
      this.editValue = taskText;
      this.tasks = this.tasks.map((task) => {
        if (task.title === taskText) {
          task.isEditing = !task.isEditing;
        }
        return task;
      });
    },

    editTask(taskText) {
      this.tasks = this.tasks.map((task) => {
        if (task.title === taskText) {
          task.isEditing = !task.isEditing;
          task.title = this.editValue;
        }
        return task;
      });
    },

    checkTaskComplete(index, type) {
      if (type === "noCompleted") {
        const complete = this.tasks.splice(index, 1);
        this.doneTasks.push(...complete);
      } else {
        const noComplete = this.doneTasks.splice(index, 1);
        this.tasks.unshift(...noComplete);
      }
    },
  },
};
</script>
