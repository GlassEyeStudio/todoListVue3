<template>
  <div>
    <h2>To do: ({{ todoElements.length }})</h2>
    <input
      type="text"
      placeholder="Type something and press ENTER"
      v-model="inputValue"
      @keyup.enter="addTodo()"
    />
    <draggable v-model="todoElements" tag="ol" item-key="uuid" group="todos">
      <template #item="{ element, i }">
        <li>
          <label>
            <input type="checkbox" v-model="element.checked" />
            <span>{{ element.text }}</span>
            <a href="#" class="remove" @click="removeTodo(i)">Remove</a>
          </label>
        </li>
      </template>
    </draggable>
    <hr />
    <h2>Completed items: ({{ todoElementsCompleted.length }})</h2>
    <draggable
      v-model="todoElementsCompleted"
      tag="ol"
      item-key="uuid"
      group="todos"
    >
      <template #item="{ element, i }">
        <li>
          <label>
            <input type="checkbox" v-model="element.checked" />
            <span>{{ element.text }}</span>
            <a href="#" class="remove" @click="removeTodo(i)">Remove</a>
          </label>
        </li>
      </template>
    </draggable>
  </div>
</template>

<script lang="ts">
import draggable from "vuedraggable";

export default {
  components: {
    draggable,
  },
};
</script>

<script lang="ts" setup>
import { v4 as uuid } from "uuid";

import { computed, ComputedRef, Ref, ref, WritableComputedRef } from "vue";

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

const todoElements: WritableComputedRef<TodoElement[]> = computed({
  get: () => todos.value.filter(({ checked }) => !checked),
  set: (value) =>
    (todos.value = [
      ...value.map((elem) => ({ ...elem, checked: false })),
      ...todoElementsCompleted.value,
    ]),
});
const todoElementsCompleted: WritableComputedRef<TodoElement[]> = computed({
  get: () => todos.value.filter(({ checked }) => checked),
  set: (value) =>
    (todos.value = [
      ...todoElements.value,
      ...value.map((elem) => ({ ...elem, checked: true })),
    ]),
});

const inputValue: Ref<string> = ref("");

const addTodo = () => {
  todos.value.push({ text: inputValue.value, uuid: uuid() });
  inputValue.value = "";
};

const removeTodo = (index: number) => {
  todos.value.splice(index, 1);
};
</script>
