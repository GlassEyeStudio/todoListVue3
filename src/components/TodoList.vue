<template>
  <h2>To do: ({{ todoElements.length }})</h2>
  <input
    type="text"
    placeholder="Type something and press ENTER"
    v-model="inputValue"
    @keyup.enter="addTodo()"
  />
  <ol>
    <li v-for="(elem, i) in todoElements" :key="elem.uuid">
      <label>
        <input type="checkbox" v-model="elem.checked" />
        <span>{{ elem.text }}</span>
        <a href="#" class="remove" @click="removeTodo(i)">Remove</a>
      </label>
    </li>
  </ol>
  <hr />
  <h2>Completed items: ({{ todoElementsCompleted.length }})</h2>
  <ol>
    <li v-for="(elem, i) in todoElementsCompleted" :key="elem.uuid">
      <label>
        <input type="checkbox" v-model="elem.checked" />
        <span>{{ elem.text }}</span>
        <a href="#" class="remove" @click="removeTodo(i)">Remove</a>
      </label>
    </li>
  </ol>
</template>

<script lang="ts" setup>
import { v4 as uuid } from "uuid";
import { computed, Ref, ref } from "vue";

interface TodoElement {
  text: string;
  checked?: boolean;
  uuid?: string;
}

const todos: Ref<TodoElement[]> = ref(
  [
    { text: "Learn about Vue" },
    { text: "Learn about Fliplet" },
    { text: "Play around in JSFiddle" },
    { text: "Show us what you've got" },
  ].map((elem) => ({
    ...elem,
    uuid: uuid(),
  }))
);

const todoElements = computed(() =>
  todos.value.filter(({ checked }) => !checked)
);
const todoElementsCompleted = computed(() =>
  todos.value.filter(({ checked }) => checked)
);

const inputValue: Ref<string> = ref("");

const addTodo = () => {
  todos.value.push({ text: inputValue.value, uuid: uuid() });
  inputValue.value = "";
};

const removeTodo = (index: number) => {
  todos.value.splice(index, 1);
};
</script>

<style scoped></style>
