<script >
import TodoList from '@/components/TodoList.vue'
export default {
  components : {
    TodoList,
  },
};
</script>

<template>
  <div id="app">
    <TodoList/>
  </div>
</template>

<style>
body {
  font-family:Arial,sans-serif;
  margin:0;
  padding:0;
}
</style>
