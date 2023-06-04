<template>
  <div class="modal" v-show="active">
    <div class="modal__wrapper">
      <v-icon class="modal__close text-subtitle-2" @click="closeTask" />
      <v-card-text
        class="modal__title text-h5 font-weight-medium text-uppercase pa-0"
        >Delete this task?
      </v-card-text>
      <div class="modal__wrapper-btn">
        <v-btn
          class="v-btn big"
          variant="tonal"
          density="comfortable"
          @click="deleteTask"
        >
          yes
        </v-btn>
        <v-btn
          class="v-btn big"
          variant="tonal"
          density="comfortable"
          @click="closeTask"
        >
          no
        </v-btn>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    active: Boolean,
    onClose: Function,
  },
  methods: {
    close() {
      this.onClose(false);
    },

    deleteTask() {
      this.$emit("deleteTask");
      this.close();
    },

    closeTask() {
      this.$emit("closeTask");
      this.close();
    },

    classToggle() {
      this.$emit("classToggle");
    },
  },
};
</script>

<style lang="scss" scoped></style>
