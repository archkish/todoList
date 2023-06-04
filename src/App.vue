<template>
  <v-app>
    <todo-list />
  </v-app>
</template>

<script>
import todoList from "@/components/todoList.vue";
export default {
  name: "App",
  components: {
    todoList,
  },
};
</script>
