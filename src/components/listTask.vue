<template>
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
            @click="showConfirmModal(index)"
          ></button>
        </div>
        <button
          class="list__btn list__btn-star"
          :class="{ 'list__btn-star__active': task.isFavourite }"
          @click="setFavourite(index)"
        >
          <svg
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            width="12"
            height="12"
            viewBox="0 0 32 32"
          >
            <path
              d="M32 12.408l-11.056-1.607-4.944-10.018-4.944 10.018-11.056 1.607 8 7.798-1.889 11.011 9.889-5.199 9.889 5.199-1.889-11.011 8-7.798z"
            ></path>
          </svg>
        </button>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
   
  },
  data() {
    return {

      }
  },

  methods: {
  
  },
};
</script>

<style lang="scss" scoped></style>
