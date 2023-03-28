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
          v-on:change="doCheck(index, 'need')"
        />
        <input
          class="list__editing"
          type="text"
          @keyup.enter="editTask(task.title)"
          v-if="task.isEditing"
          v-model="editValue"
        >
        <span
          else
          type="text"
          class="list__task"
          :class="{ completed: task.completed, isEditing: task.isEditing }"
        >
          {{ task.title }}
        </span>
        <div class="wrapper__btn">
          <button
            class="list__btn list__btn_correction"
            @click="changeEditing(task.title)"
          ></button>
          <button class="list__btn" @click="deleteTask(index)"></button>
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
          v-on:change="doCheck(index, 'complete')"
          v-model="task.completed"
        />
        <span class="list__show" :class="{ completed: task.completed }">{{
          task.title
        }}</span>
        <div class="wrapper__btn">
          <!-- <button class="list__btn list__btn_correction"></button> -->
          <button class="list__btn" @click="deleteDoneTask(index)"></button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      // let newTask = "";
      newTask: "",
      editValue: "",
      // let tasks = [{...}]
      tasks: [
        {
          title: "add styles by design",
          completed: false,
          isEditing: false,
        },
        {
          title: "add sort by complete",
          completed: false,
          isEditing: false,
        },
        {
          title: "watch video about start vue3 install",
          completed: false,
          isEditing: false,
        },
        {
          title: "add tasks in localStorage",
          completed: false,
          isEditing: false,
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
      handler(nlist) {
        localStorage.doneTasks = JSON.stringify(nlist);
      },
      deep: true,
    },
  },

  methods: {
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
    edit() {},
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

    doCheck(index, type) {
      if (type === "need") {
        const complete = this.tasks.splice(index, 1);
        this.doneTasks.push(...complete);
      } else {
        const nocomplete = this.doneTasks.splice(index, 1);
        this.tasks.unshift(...nocomplete);
      }
    },
  },
};
</script>
