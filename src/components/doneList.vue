<template>
  <div class="wrapper-donelist">
    <div @click="checkTaskComplete">
      <v-checkbox class="checked" />
    </div>
    <span class="completed">{{ title }}</span>
    <div class="wrapper__btn">
      <v-icon
        class="list__btn text-subtitle-1"
        @click="deleteDoneTask"
      ></v-icon>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selected: true,
    };
  },
  props: {
    doneTasks: Array,
    title: String,
  },
  methods: {
    deleteDoneTask() {
      this.$emit("deleteDoneTask");
    },
    checkTaskComplete() {
      this.$emit("checkTaskComplete");
    },
  },
};
</script>

<style lang="scss" scoped></style>
