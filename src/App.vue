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
          @change="checkTaskComplete(index, true)"
        />
        <input
          class="list__editing"
          type="text"
          @keyup.enter="saveEditedTask(task)"
          v-if="task.isEditing"
          v-model="editValue"
        />
        <span
          v-else
          class="list__task"
          :class="{ completed: task.completed, isEditing: task.isEditing }"
        >
          {{ task.title }}
        </span>
        <div class="wrapper__btn">
          <div class="wrapper">
            <button
              class="list__btn check"
              v-if="task.isEditing"
              @click="saveEditedTask(task)"
            ></button>
            <button
              v-else
              class="list__btn list__btn_correction"
              @click="editTask(task)"
            ></button>
          </div>
          <div>
            <button
              class="list__btn list__btn_close"
              v-if="task.isEditing"
              @click="closeEditing(task)"
            ></button>
            <button
              v-else
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
          @change="checkTaskComplete(index, false)"
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

  beforeUnmount() {
    this.disableEditing();
  },

  mounted() {
    if (localStorage.doneTasks) {
      this.doneTasks = JSON.parse(localStorage.doneTasks);
    }

    if (localStorage.tasks) {
      this.tasks = JSON.parse(localStorage.tasks);
    }

    window.onbeforeunload = () => {
      this.disableEditing();
    };
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

    disableEditing() {
      this.tasks.forEach((item) => {
        item.isEditing = false;
      })
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    deleteDoneTask(index) {
      this.doneTasks.splice(index, 1);
    },

    editTask(task) {
      this.disableEditing();
      this.editValue = task.title;
      task.isEditing = true;
    },

    closeEditing(task) {
      this.editValue = '';
      task.isEditing = false;
    },

    saveEditedTask(task) {
      task.isEditing = false;
      task.title = this.editValue;
    },

    checkTaskComplete(index, isCompleted) {
      if (isCompleted) {
        const complete = this.tasks.splice(index, 1);
        this.doneTasks.push(...complete);
      } else {
        const noComplete = this.doneTasks.splice(index, 1);
        this.tasks.push(...noComplete);
      }
    },
  },
};
</script>
