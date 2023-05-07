<template>
    <div class="modal">
      <div class="modal__wrapper">
        <div class="modal__close"
        @click="classToggle"
        >&times;</div>
        <div class="modal__title">Delete this task?</div>
        <div class="modal__wrapper-btn">
          <button 
          class="form__btn form__btn_big"
          @click="deleteTask">yes</button>
          <button
           class="form__btn form__btn_big"
           @click="closeTask">no</button>
        </div>
      </div>
    </div>
</template>

<script>

    export default {
        data () {
            return {  
            }
        },
        methods: {
            deleteTask() {
                this.$emit('deleteTask')
            },

            closeTask() {
                this.$emit('closeTask')
            },
            classToggle() {
                this.$emit('classToggle')
            },
        },
    
    }
</script>

<style lang="scss" scoped>

</style>