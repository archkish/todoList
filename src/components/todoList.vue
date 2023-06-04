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
    <v-card-text class="text-h4 font-weight-bold pa-0">ToDo List </v-card-text>
    <v-form class="form" @submit.prevent="addTask">
      <v-text-field
        class="form"
        type="text"
        v-model="newTask"
        label="Add a new task..."
        variant="underlined"
      />
      <v-btn
        class="small"
        variant="tonal"
        density="comfortable"
        @click="addTask"
      >
        Add Task
      </v-btn>
    </v-form>

    <section class="wr">
      <ul class="list">
        <li class="list__item" v-for="task in tasks" :key="task.id">
          <list-task
            @setFavourite="setFavourite(task)"
            @click="overlay = !overlay"
            @showConfirmModal="showConfirmModal(task.id)"
            @editTask="editTask(task)"
            @closeEditing="closeEditing(task)"
            @saveEditedTask="saveEditedTask(task)"
            @checkTaskComplete="checkTaskComplete(task.id, true)"
            @noteValueChange="editValue = $event"
            :task="task"
            :noteValue="editValue"
          />
        </li>
      </ul>
    </section>

    <v-divider class="border-opacity-50"></v-divider>
    <v-card-text class="text-h5 font-weight-bold pl-0">Done</v-card-text>

    <section>
      <ul class="list" id="done">
        <li class="list__item" v-for="task in doneTasks" :key="task.id">
          <done-list
            :doneTasks="doneTasks"
            :title="task.title"
            :completed="task.completed"
            @deleteDoneTask="deleteDoneTask(task.id)"
            @checkTaskComplete="checkTaskComplete(task.id, false)"
          />
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
        indexCounter: 4,
        messages: [
        {
          from: 'You',
          message: `Sure, I'll see you later.`,
          time: '10:42am',
          color: 'deep-purple-lighten-1',
        },
        {
          from: 'John Doe',
          message: 'Yeah, sure. Does 1:00pm work?',
          time: '10:37am',
          color: 'green',
        },
        {
          from: 'You',
          message: 'Did you still want to grab lunch today?',
          time: '9:47am',
          color: 'deep-purple-lighten-1',
        },
      ],
        tasks: [
          {
            id: 0,
            title: "add styles by design",
            completed: false,
            isEditing: false,
            isFavourite: false,
          },
          {
            id: 1,
            title: "add sort by complete",
            completed: false,
            isEditing: false,
            isFavourite: false,
          },
          {
            id: 2,
            title: "watch video about start vue3 install",
            completed: false,
            isEditing: false,
            isFavourite: false,
          },
          {
            id: 3,
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
      indexCounter(value) {
          localStorage.indexCounter = JSON.stringify(value);
      },
    },

    methods: {
      addTask() {
        if (this.newTask.trim() !== "") {
          this.indexCounter++;
          this.tasks.push({
            id: this.indexCounter,
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

      getTaskIndexById(id) {
        return this.tasks.findIndex((item) => item.id === id);
      },

      getDoneTaskIndexById(id) {
        return this.doneTasks.findIndex((item) => item.id === id);
      },

      setFavourite(task) {
        const index = this.getTaskIndexById(task.id);

        task.isFavourite = !task.isFavourite;

        if (task.isFavourite) {
          const [item] = this.tasks.splice(index, 1);
          this.tasks.unshift(item)
        } else {
          const lastFavourite = this.tasks.filter((el) => el.isFavourite).at(-1);

          if (lastFavourite !== undefined) {
            const lastFavouriteIndex = this.getTaskIndexById(lastFavourite.id);

            const [item] = this.tasks.splice(index, 1);

            this.tasks.splice(index < lastFavouriteIndex ? lastFavouriteIndex : lastFavouriteIndex + 1, 0, item)
          }
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

      deleteDoneTask(id) {
        const index = this.getDoneTaskIndexById(id);
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

      checkTaskComplete(id, isCompleted) {
        if (isCompleted) {
          const taskIndex = this.getTaskIndexById(id);
          const [completedTask] = this.tasks.splice(taskIndex, 1);
          this.doneTasks.push(completedTask);
        } else {
          const taskIndex = this.getDoneTaskIndexById(id);
          const [noCompletedTask] = this.doneTasks.splice(taskIndex, 1);

          const lastFavourite = this.tasks.filter((el) => el.isFavourite).at(-1);

          if (lastFavourite !== undefined) {
            const lastFavouriteIndex = this.getTaskIndexById(lastFavourite.id);

            this.tasks.splice( lastFavouriteIndex + 1, 0, noCompletedTask);
          } else {
            this.tasks.unshift(noCompletedTask);
          }
        }
      },


      showConfirmModal(id) {
        const index = this.getTaskIndexById(id);
        this.deletedTaskIndex = index;
        this.active = !this.active;
      },
    },

}
</script>
