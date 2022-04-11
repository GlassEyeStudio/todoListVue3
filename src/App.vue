<template>
  <TodoList />
</template>

<script lang="ts" setup>
import TodoList from "./components/TodoList.vue";
</script>

<style>
body {
  padding: 20px;
  font-family: Helvetica;
}

li {
  margin: 8px 0;
}

h2 {
  font-weight: bold;
  margin-bottom: 15px;
}

del {
  color: rgba(0, 0, 0, 0.3);
}

input[type="text"] {
  padding: 10px;
  width: 200px;
}

label .remove {
  display: none;
  color: red;
}

label:hover .remove {
  display: inline-block;
  margin-left: 10px;
  color: red;
  opacity: 0.5;
}
</style>
