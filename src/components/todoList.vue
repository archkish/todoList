<template>
  <div class="tasks">
    <modal-window
      v-model="active"
      @closeTask="closeTask"
      @deleteTask="deleteTask"
      :deletedTaskIndex="deletedTaskIndex"
      :active="active"
      :on-close="onConfirmClose"
    />
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

    <section>
      <ul class="list">
      <li class="list__item" v-for="(task, index) in tasks" :key="index">
        <list-task
          @setFavourite="setFavourite(index)"
          @showConfirmModal="showConfirmModal(index)"
          @editTask="editTask(task)"
          @closeEditing="closeEditing(task)"
          @saveEditedTask="saveEditedTask(task)"
          @checkTaskComplete="checkTaskComplete(index, true)"
          @noteValueChange="editValue = $event"

          :noteValue="editValue"
          :title="task.title"
          :completed="task.completed"
          :isEditing="task.isEditing"
          :isFavourite="task.isFavourite"        
        />
      </li>
    </ul>
    </section>

    <hr class="devider" />
    <h2 class="tasks__title tasks__title_left">Done</h2>

    <section>
      <ul class="list" id="done">
      <li class="list__item" v-for="(task, index) in doneTasks" :key="index">
        <done-list
        :doneTasks="doneTasks"
        :title="task.title"
        :completed="task.completed"
        @deleteDoneTask="deleteDoneTask"
        @checkTaskComplete="checkTaskComplete"/>
      </li>
    </ul>
  </section>
  </div>
</template>

<script lang="js">
import modalWindow from "@/components/modalWindow.vue"
import listTask from "@/components/listTask.vue"
import doneList from "@/components/doneList.vue"
export default {
    components: {
      modalWindow,
      listTask,
      doneList
    },
    name: "App",
    data() {
      return {
        newTask: "",
        editValue: "",
        doneTasks: [],
        active: false,
        deletedTaskIndex: null,
        tasks: [
          {
            title: "add styles by design",
            completed: false,
            isEditing: false,
            isFavourite: false,
          },
          {
            title: "add sort by complete",
            completed: false,
            isEditing: false,
            isFavourite: false,
          },
          {
            title: "watch video about start vue3 install",
            completed: false,
            isEditing: false,
            isFavourite: false,
          },
          {
            title: "add tasks in localStorage",
            completed: false,
            isEditing: false,
            isFavourite: false,
          },
        ],
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
          this.tasks.push({
            title: this.newTask,
            completed: false,
            isEditing: false,
            isFavourite: false,
          });

          this.newTask = "";
        }
      },

      disableEditing() {
        this.tasks.forEach((item) => {
          item.isEditing = false;
        });
      },

      setFavourite(index) {
        const [task] = this.tasks.splice(index, 1);
        task.isFavourite = !task.isFavourite;

        if(task.isFavourite) {
          this.tasks.unshift(task)
        } else {
          this.tasks.push(task);
        }
      },

      onConfirmClose(value) {
        if(value) {
          this.active = true
        } else {
          this.active = false 
        }
      },


      deleteTask() {
        this.tasks.splice(this.deletedTaskIndex, 1);
        this.active = !this.active;
        this.deletedTaskIndex = null;
      },

      classToggle() {
        this.active = !this.active
      },

      closeTask() {
        this.active = !this.active
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
        this.editValue = "";
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


      showConfirmModal(index) {
        this.deletedTaskIndex = index;
        this.active = !this.active;
      },
    },

  };
</script>
