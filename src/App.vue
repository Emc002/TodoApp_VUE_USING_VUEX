<template>
  <div id="app">
    <div class="navbar"><h1>TODO APP</h1></div>
    <todolist></todolist>
  </div>
</template>

<script>
import todolist from './components/todolist.vue';

export default {
  name: "App",
  components: {
    todolist,
  },
  
};
</script>