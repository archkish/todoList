<template>
  <div class="tasks">
    <div class="modal" :class="{hide: !active}">
      <div class="modal__wrapper">
        <div class="modal__close"
        @click="active = !active"
        >&times;</div>
        <div class="modal__title">Delete this task?</div>
        <div class="modal__wrapper-btn">
          <button 
          class="form__btn form__btn_big"
          @click="deleteTask(index)">yes</button>
          <button
           class="form__btn form__btn_big"
           @click="closeTask(index)">no</button>
        </div>
      </div>
    </div>
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
      <li class="list__item" v-for="(task, index) in chosenTasks" :key="index">
        <input
          class="list__checkbox"
          type="checkbox"
          v-model="task.completed"
          @change="checkTaskComplete2(index, true)"
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
              class="list__btn list__btn-correction"
              @click="editTask(task)"
            ></button>
          </div>
          <div>
            <button
              class="list__btn list__btn-close"
              v-if="task.isEditing"
              @click="closeEditing(task)"
            ></button>
            <button
              v-else
              class="list__btn"
              @click="active = !active"
            ></button>
          </div>
          <button
            class="list__btn list__btn-star star2"
            @click="makeChosen(index, false)"
          >
          <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 32 32">
            <path d="M32 12.408l-11.056-1.607-4.944-10.018-4.944 10.018-11.056 1.607 8 7.798-1.889 11.011 9.889-5.199 9.889 5.199-1.889-11.011 8-7.798z"></path>
          </svg>
          </button>
        </div>
      </li>
    </ul>
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
              class="list__btn list__btn-correction"
              @click="editTask(task)"
            ></button>
          </div>
          <div>
            <button
              class="list__btn list__btn-close"
              v-if="task.isEditing"
              @click="closeEditing(task)"
            ></button>
            <button
              v-else
              class="list__btn"
              @click="active = !active"
            ></button>
          </div>
          <button
            class="list__btn list__btn-star"
            @click="makeChosen(index, true)"
          ></button>
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
      chosenTasks: [],
      active: false,
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

    if (localStorage.chosenTasks) {
      this.chosenTasks = JSON.parse(localStorage.chosenTasks);
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
    chosenTasks: {
      handler(newChosenTasks) {
        localStorage.chosenTasks = JSON.stringify(newChosenTasks)
      },
      deep: true,
    }
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
      });
    },

    disableEditing2() {
      this.chosenTasks.forEach((item) => {
        item.isEditing = false;
      });
    },

    deleteTask(index) {
      this.tasks.splice(index, 1)
      this.active = !this.active
    },

    deleteTask2(index) {
      this.chosenTasks.splice(index, 1);
    },

    closeTask() {
      this.active = !this.active
    },

    deleteDoneTask(index) {
      this.doneTasks.splice(index, 1);
    },

    editTask(task) {
      this.disableEditing();
      this.disableEditing2();
      this.editValue = task.title;
      task.isEditing = true;
    },

    closeEditing(task) {
      this.editValue = "";
      task.isEditing = false;
    },

    makeChosen(index, isCompleted) {
      if (isCompleted) {
        const chose = this.tasks.splice(index, 1);
        this.chosenTasks.unshift(...chose);
      } else {
        const noChose = this.chosenTasks.splice(index, 1);
        this.tasks.unshift(...noChose);
      }
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

    checkTaskComplete2(index) {
      const complete = this.chosenTasks.splice(index, 1);
      this.doneTasks.push(...complete);
    },
  },
};
</script>
