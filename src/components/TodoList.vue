<template>
  <ul class="list-group">
    <TodoListItem
      v-for="todoItem in todoList"
      :key="todoItem.id"
      :todoItem="todoItem"
      @delete-todo="emit('delete-todo', $event)"
      @toggle-completed="emit('toggle-completed', $event)"
      @update-due-date="emit('update-due-date', $event)"
    />
  </ul>
</template>

<script setup>
import TodoListItem from "./TodoListItem.vue";

// TodoListItem -> TodoList -> App 토스
defineProps({
  todoList: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits([
  "delete-todo",
  "toggle-completed",
  "update-due-date",
]);
</script>
