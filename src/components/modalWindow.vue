<template>
    <div class="modal" v-show="active">
      <div class="modal__wrapper">
        <div class="modal__close"
        @click="closeTask"
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
        props: {
            active: Boolean,
            onClose: Function,
     
        },
        methods: {
            close() {
                this.onClose(false)
            },


            deleteTask() {
                this.$emit('deleteTask')
                this.close()
            },

            closeTask() {
                this.$emit('closeTask')
                this.close()

            },

            classToggle() {
                this.$emit('classToggle')
            },
        },
    
    }
</script>

<style lang="scss" scoped>

</style>