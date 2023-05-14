<template>
  <div class="wrapp">
    <input
      type="checkbox"
      class="list__checkbox"
      @change="checkTaskComplete"
    />
    <input
      class="list__editing"
      type="text"
      @keyup.enter="saveEditedTask"
      @input="changeTextValue"
      v-if="isEditing"
      :value="noteValue"
    />
    <span
      v-else
      class="list__task"
      :class="{ completed: completed, isEditing: isEditing }"
    >
    {{ title }}
    </span>

    <div class="wrapper__btn wrapp__btn">
      <div class="wrapper">
        <button
          class="list__btn check"
          v-if="isEditing"
          @click="saveEditedTask"
        ></button>
        <button
          v-else
          class="list__btn list__btn-correction"
          @click="editTask"
        ></button>
      </div>
      <div>
        <button
          class="list__btn list__btn-close"
          v-if="isEditing"
          @click="closeEditing"
        ></button>
        <button
          v-else
          class="list__btn"
          @click="showConfirmModal"
        ></button>
      </div>
      <button
        class="list__btn list__btn-star"
        :class="{ 'list__btn-star__active': isFavourite }"
        @click="setFavourite"
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
  </div>
 
    
</template>

<script>

export default {

  props: {
    noteValue: String,
    title: String,
    completed: Boolean,
    isEditing: Boolean,
    isFavourite: Boolean,
  },

  data() {
    return {
      textValue: ""
    };
  },
  methods: {
    changeTextValue(event) {
      this.textValue = event.target.value
      this.$emit('noteValueChange', this.textValue)
    },
    showConfirmModal() {
      this.$emit('showConfirmModal')
    },
    setFavourite() {
      this.$emit('setFavourite')
    },
    editTask() {
      this.$emit('editTask')
    },
    closeEditing() {
      this.$emit('closeEditing')
    },
    saveEditedTask() {
      this.$emit('saveEditedTask')
    },
    
    checkTaskComplete() {
      this.$emit('checkTaskComplete')
    }
  }
};
</script>

<style lang="scss" scoped>
.wrapp {
  display: flex;
  width: 100%;
}

</style>
