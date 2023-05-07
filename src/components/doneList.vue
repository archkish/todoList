<template>
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
</template>

<script>
    export default {
        
    }
</script>

<style lang="scss" scoped>

</style>