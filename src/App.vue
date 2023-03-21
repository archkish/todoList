<template>
  <div class="tasks">
    <h2 class="tasks__title">ToDo List</h2>
    <form class="form" @submit.prevent="addTask">
      <input class="form__input" type="text" v-model="newTask" placeholder="Add a new task...">
      <button class="form__btn">Add Task</button>
    </form>
    <ul class="list">
      <li class="list__item" v-for="(task, index) in tasks" :key="index">
        <input class="list__checkbox" type="checkbox" v-model="task.completed">
        <span class="list__task" :class="{ completed: task.completed }">{{ task.title }}</span>
        <button class="list__btn" @click="deleteTask(index)"></button>
      </li>
    </ul>
  </div>

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


  mounted() {
    if(localStorage.tasks) {
      this.tasks = JSON.parse(localStorage.tasks)
    }
  },

  watch: {
    tasks: {
      handler(newList) {
        localStorage.tasks = JSON.stringify(newList)
      },
      deep: true
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

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400&display=swap');
  * {
    font-family: 'Roboto';
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  #app {
    height: 100vh;
    background: rgba($color: #302f2f, $alpha: 0.5);
    margin-right: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .tasks {
    min-width: 400px;
    background: rgba($color: #ede8e8, $alpha: 0.5);
    border-radius: 8px;
    padding: 30px ;
    &__title {
      text-align: center;
      
    }
  }
  .form {
    margin-top: 15px;
    width: 100%;
    &__input {
      width: 77%;
      height: 30px;
      font-size: 18px;
      color: #5e5c5c;
      background: none;
      outline: none;
      border: none;
      border-bottom: 1px solid #5e5c5c;
      &::placeholder {
        font-size: 14px;
      }
    }
    &__btn {
      width: 20%;
      height: 30px;
      margin-left: 10px;
      border: none;
      background: #d6d2d2;
      cursor: pointer;
      border-radius: 8px;
      font-size: 12px;
      color: #5e5c5c;
      transition: 0.4s;
      &:hover {
        box-shadow: 0 0 0 2px #9f9b9b;
      }
    }
  }
  .list {
    margin-top: 15px;
    list-style-type: none;
    font-size: 18px;
    line-height: 18px;
    color: #5e5c5c;
    &__item {
      min-height: 30px;
      margin-top: 8px;
      padding: 5px 5px;
      display: flex;
      align-items: center;
      background: #d6d2d2;
      border-radius: 4px;
      input {
        margin-right: 5px;
        width: 16px;
        height: 16px;
        border: 1px solid #5e5c5c;
        background: url(./assets/checkmark2.png) center center no-repeat;
        background-size: 0;
        transition: 0.2s;
        border-radius: 50%;
        appearance: none;
        &:hover {
          border-color: #dc8e8e; 
          box-shadow: 0 0 0 1px #efa5a5;
        }
        &:checked {
          background-size: 11px;
        }
      }
    }
    &__task {
      max-width: 370px;
    }
    &__btn {
      margin-left: auto;
      width: 16px;
      height: 16px;
      border: none;
      cursor: pointer; 
      transition: 0.4s;
      background: url(./assets/trash.png) center center/cover no-repeat;
      &:hover {
        transform: scale(1.1);
      }
    }
  }

  
  .completed {
    text-decoration: line-through;
    }
</style>
