<template>
  <div class="wrapper">
    <input
          class="list__checkbox"
          type="checkbox"
          checked
          @change="checkTaskComplete"
        />
        <span class="completed">{{
          title
        }}</span>
        <div class="wrapper__btn">
          <button class="list__btn" @click="deleteDoneTask"></button>
        </div>
  </div>
</template>

<script>
    export default {
      data() {
        return {

        }
      },
      props: {
        doneTasks: Array,
        title: String,
        
      },
      methods: {
        deleteDoneTask() {
          this.$emit('deleteDoneTask')
        },
        checkTaskComplete() {
          this.$emit('checkTaskComplete')
        },
      }
    }

</script>

<style lang="scss" scoped>
  .wrapper {
    display: flex;
    width: 100%;
  }
  .completed {
    text-decoration: line-through;
  }
</style>